# Sürüm notları / release notes

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
