# Sürüm notları / release notes

## v0.9.0 — Ağlar arası USDC (Circle CCTP) / cross-chain USDC (15 Ekim 2026)

- **Ağlar arası USDC:** Circle CCTP V2 ile saklamasız transfer (Ethereum Sepolia, Avalanche Fuji, Arbitrum Sepolia, Polygon Amoy). Kullanıcının cüzdanı tam tutarlı onay, yakma ve basma adımlarını ayrı ayrı imzalar; İmza Koruması yalnızca sabitlenmiş Circle kontratlarına giden bu üç çağrıyı, ekranda onaylanan değerlerden yeniden üretilen veriyle bayt bayt eşleşirse gönderir.
- **Sunucu doğrulaması:** yakma işlemi zincirden, Circle onayı (attestation) yakılan mesajla alan alan, basma işlemi hedef zincirden doğrulanır; kayıtlar SQL Server'da tutulur. Canlı ortamda gerçek test ağı transferleriyle (Sepolia → Fuji, Sepolia → Arbitrum Sepolia) doğrulandı.
- **Test:** gerçek Circle kontratlarını çalıştıran Sepolia ve Fuji çatallarında uçtan uca tarayıcı testi (12,5 USDC yakıldı ve basıldı).
- **Cross-chain USDC** via Circle CCTP V2 (non-custodial, testnets) with Signing Guard support and server-side verification.

## v0.8.0 — E-ticaret API'si, kasa ekranı ve güvenlik incelemesi / merchant API, POS and security review (15 Ekim 2026)

- **E-ticaret API'si:** çalışma alanına bağlı API anahtarları (`mpk_test_…`, SHA-256 özetiyle saklanır, anında iptal); sunucudan fatura/çok ağlı ödeme açma, durum, mutabakat ve CSV; ödeme sonrası mağazaya dönüş bağlantısı. Canlı ortamda uçtan uca doğrulandı.
- **Kasa ekranı (POS):** tutar gir → tek QR → ödeme kesinleşince onay.
- **Güvenlik incelemesi:** dört bağımsız inceleme ajanı (backend, cüzdan, altyapı, uyum kanıtı). 2 yüksek bulgu (ödeme ipucuyla fatura kaydırma; doğrulanmış satıcı varsayımının benzer adres kontrolünü atlaması) ve 9 orta bulgunun 6’sı düzeltildi (kalan 3 altyapı maddesi takipte); regresyon testleri eski kodda başarısız oluyor. CI eylemleri SHA ile sabitlendi, tüm imajlar Trivy ile taranıyor, Dependabot açık.
- **Merchant API**, POS screen, multi-agent security review with fixes; SOC 2 / ISO 27001 control matrix verified row by row.

## v0.7.0 — Çok ağlı ödeme, Avalanche ve uyum hazırlığı / multi-network checkout, Avalanche, compliance readiness (15 Ekim 2026)

- **Çok ağlı ödeme:** tek fatura birden fazla ağda; müşteri ağı seçer, ilk ödeme diğerlerini kapatır (aynı varlık ya da USDC/USDT 1:1, kur yok). Canlı ortamda BNB Smart Chain'de ödenip Avalanche seçeneğinin kapandığı doğrulandı.
- **Avalanche C-Chain (Fuji):** AVAX ve USDC.
- **Dayanıklılık:** paylaşımlı barındırmada BSC log taramasının takıldığı hata bulundu ve düzeltildi (uyarlanabilir blok aralığı, sağlayıcı sınırı hatasında yedeğe geçiş, yeni sağlayıcılar).
- **Uyum hazırlığı:** SOC 2 ve ISO/IEC 27001:2022 kontrol matrisi (kanıtlarıyla), politika taslakları, risk kaydı, olay müdahale planı; PCI DSS kapsam dışı beyanı.
- **Multi-network checkout**, Avalanche Fuji, BSC log-scanning fix on shared hosting, SOC 2 / ISO 27001 readiness pack.

## v0.6.0 — BNB Smart Chain, KVKK ve Lite / BNB Smart Chain, privacy and Lite (15 Ekim 2026)

- **BNB Smart Chain Testnet:** BNB, USDT ve USDC (18 ondalık) ile ödeme; hızlı bloklar için paralel blok tarama; 15 onay.
- **Hesap silme (KVKK md. 7 / GDPR md. 17):** kişisel veriler anonimleştirilir, mali kayıtlar yasal saklama için korunur; son sahip koruması.
- **Yasal sayfalar:** aydınlatma metni, kullanım koşulları, çerez bilgisi (hukuki inceleme bekleyen taslak olarak işaretli).
- **Lite paketi:** web arayüzü, API ve zincir izleyici tek süreçte; Redis/RabbitMQ gerekmez; IIS/FTPS dağıtım betiği.
- **BNB Smart Chain testnet**, account erasure, legal pages, single-process Lite package for small and free hosts.

## v0.5.0 — İmza Koruması / Signing Guard (14 Ekim 2026)

- **İmza Koruması:** imzalanan işlem cihazda çözülür; ağ, alıcı, tutar, token, ücret, nonce ve imzacı incelenen değerlerle karşılaştırılır, fark varsa gönderilmez. Yalnızca düz transfer imzalanır (approve, gizli çağrı, yetki devri yok).
- **Alıcı kontrolleri:** doğrulanmış satıcı ve bilinen adreste ek adım yok; yeni alıcıda son 4, adres zehirleme şüphesinde son 8 karakter; sıfır adres ve token kontratı reddedilir; adresler 4'lü gruplar halinde.
- **İkonlar ve PWA:** yeni ikon seti (favicon.ico, SVG, iOS, maskable), telefona kurulum yeniden çalışıyor; Öde / Talep / Cüzdan kısayolları.
- **Altyapı:** RPC sağlayıcı yedeklemesi (zincir kimliği doğrulamalı), otomatik HTTPS'li tek sunucu kurulumu, eşzamanlı kayıt hatası ve eksik güvenlik başlıkları düzeltildi.
- **Signing Guard:** signed bytes are checked against the reviewed values before broadcast; plain transfers only. Recipient checks, new icon set and installable PWA, RPC failover, single-server HTTPS bundle.

## v0.4.0 — Kurulum ve dayanıklılık / install and resilience (14 Ekim 2026)

- Tek sunucu kurulumu (Docker Compose + Caddy, otomatik Let's Encrypt), yedek RPC sağlayıcıları, yeni doğal seslendirmeli tanıtım videosu.

## v0.3.0 — Muhasebe dışa aktarma / accounting exports (14 Ekim 2026)

- **CSV dışa aktarma:** ödeme talepleri ve defter yevmiyesi; tutarlar hem en küçük birimde hem tam ondalık olarak (kayan nokta yok), iade ve net tutar, tarih aralığı, UTF-8 BOM, formül enjeksiyonuna karşı koruma. Organizasyonlarda Finans ve üstü roller; her dışa aktarma denetim kaydına yazılır.
- **CSV exports:** payment requests and the ledger journal with exact base-unit and decimal amounts, refunds and net, date range, formula-injection safe; Finance+ and audited.

## v0.2.0 — İşletme özellikleri / business features (14 Ekim 2026)

- **Organizasyonlar:** Sahip / Yönetici / Finans / İzleyici rolleri, e-posta ile davet, ekip çalışma alanında ortak ödeme talepleri, defter ve webhook'lar. Cüzdan anahtarları kişide kalır.
- **İadeler:** satıcı iadeyi kendi cüzdanında imzalar; iade, tam transfer zincirde bulunup onay derinliğine ulaşınca onaylanır ve deftere işlenir (Dr 4090 İadeler / Cr 1010).
- **Webhook'lar:** HMAC-SHA256 imzalı olaylar, en az bir kez teslim, artan aralıklarla yeniden deneme, teslimat günlüğü, SSRF koruması, şifreli sırlar.
- **Hesap güvenliği:** e-posta doğrulama, şifre sıfırlama, kimlik doğrulayıcı uygulamayla iki adımlı doğrulama (TOTP) ve tek kullanımlık kurtarma kodları.
- **Harici cüzdanlar:** MetaMask ya da başka bir cüzdandan gönderilen native ETH ödemeleri de işlem hash'i gerekmeden algılanır.
- **Kubernetes:** Helm chart (API, worker, web, migration job, HPA, PDB, NetworkPolicy, kısıtlı Pod Security) — kind kümesinde production modunda doğrulandı.

## v0.1.0 — Ödeme dikey dilimi / payment vertical slice

- Kendi anahtarınızı tuttuğunuz (non-custodial) tarayıcı cüzdanı: BIP-39, Argon2id + AES-256-GCM şifreli kasa.
- QR ödeme talepleri (modPAY bağlantısı ve EIP-681), kamera ile okuma, ödeyen ekranı.
- Zincir izleyici, onay derinliği, reorg tespiti; SQL Server'da çift taraflı, değiştirilemez defter ve mutabakat.
- Transactional outbox → RabbitMQ → SignalR canlı güncellemeler; Redis lider kilidi; Hangfire işleri.
