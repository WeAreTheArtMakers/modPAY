# Kurulum ve işletim

## Seçenekler

- **Barındırılan testnet pilotu:** WATAM'ın işlettiği ortamda, kendi alan adınız ve webhook uç noktanızla.
- **Kendi altyapınız (Kubernetes):** Helm chart ile API, Worker ve Web; SQL Server/Azure SQL, Redis ve RabbitMQ yönetilen servis olarak.
- **Tek sunucu (Docker Compose + otomatik HTTPS):** küçük pilotlar ve tanıtım ortamları. Tek komutla SQL Server Express, Redis, RabbitMQ, API, Worker ve web kurulur; Caddy, alan adınız için Let's Encrypt sertifikasını kendisi alır. En az 4 GB RAM'li bir Linux sunucu ve bir DNS kaydı yeterlidir.

## Tek sunucu: özet

1. Linux sunucu (x86-64, ≥ 4 GB RAM), Docker kurulu; 80/443 açık.
2. DNS: `pay.sirketiniz.com` → sunucu IP adresi.
3. Ortam dosyasında alan adı, Let's Encrypt e-postası, rastgele şifreler, SMTP ve RPC sağlayıcısı.
4. `docker compose ... up -d --build` — şema kurulumu otomatik; birkaç dakika içinde `https://pay.sirketiniz.com` açılır.

Doğrulanmış: paket production modunda yerel olarak kuruldu; tüm yanıtlarda HSTS/CSP dahil 7 güvenlik başlığı, cüzdan, CSP ve iki adımlı doğrulama tarayıcı testleri masaüstü ve mobilde geçti.

> Kalıcı yayında geçici tünel adresleri (ör. `trycloudflare.com`) kullanmayın: cüzdan sayfaları barındıran rastgele alan adları tarayıcılarca "tehlikeli site" olarak işaretlenebilir. Kendi alan adınız ve geçerli sertifika gerekir.

## Gereksinimler (Kubernetes)

| Bileşen | Öneri |
|---|---|
| Kubernetes | 1.29+; ingress denetleyicisi ve TLS sertifikası (ör. cert-manager) |
| SQL Server | SQL Server 2022 ya da Azure SQL Database |
| Redis | Azure Cache for Redis ya da eşdeğeri |
| RabbitMQ | Yönetilen ya da küme halinde RabbitMQ 4.x |
| SMTP | E-posta doğrulama, şifre sıfırlama ve davetler için |
| RPC | Ağ başına bir sağlayıcı URL'si (anahtarlar sunucuda kalır) |

Kaynaklar (başlangıç): API 2 × (100m CPU, 192 MiB), Worker 1 × (100m, 192 MiB), Web 2 × (20m, 32 MiB); API otomatik ölçeklenir.

## Kurulum adımları

1. Konteyner imajları kayıt defterinize aktarılır.
2. Bağlantı dizeleri bir gizli anahtar deposundan (Key Vault, External Secrets) Kubernetes Secret olarak verilir.
3. `helm upgrade --install` — veritabanı migration'ları kurulum öncesi iş olarak çalışır.
4. Kabul testi: kayıt, cüzdan, sahiplik doğrulama, test ağında ödeme, defter mutabakatı, webhook.

Doğrulanmış: Helm chart, yerel bir kind kümesinde production modunda kuruldu; tarayıcı testleri (kayıt, cüzdan, sahiplik imzası, iki adımlı doğrulama, CSP) masaüstü ve mobilde geçti.

## İşletim

- Sağlık uç noktaları: `/health/live`, `/health/ready`; zincir izleyici gecikmesi `/api/chains/status`.
- OpenTelemetry izleri ve metrikleri (OTLP), yapılandırılmış loglar (hassas veriler maskelenir).
- Hangfire ile saklama işleri; webhook teslimat günlüğü panelde.
