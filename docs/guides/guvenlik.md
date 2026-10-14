# Güvenlik modeli

## Anahtarlar

- Kurtarma ifadesi ve özel anahtarlar **yalnızca kullanıcının cihazında** üretilir ve saklanır: Argon2id (64 MiB) ile türetilen anahtarla AES-256-GCM şifreleme, IndexedDB'de.
- Cüzdan otomatik kilitlenir; dışa aktarma her seferinde cüzdan şifresi ister; kurtarma ifadesi panoya otomatik kopyalanmaz.
- Sunucuya yalnızca imzalı işlemler ve herkese açık adresler gider. Uçtan uca testler, kurtarma ifadesinin hiçbir ağ isteğinde yer almadığını doğrular.
- İşlem, kullanıcı ağ, alıcı, tutar ve azami ücreti gördükten ve açıkça onayladıktan sonra imzalanır.

## Hesap ve oturum

- ASP.NET Core Identity; HttpOnly, Secure, SameSite=Strict, `__Host-` önekli oturum çerezi; CSRF koruması.
- Kilitleme, kullanıcı/IP başına istek sınırları, hesap varlığını sızdırmayan hata mesajları.
- E-posta doğrulama, tek kullanımlık şifre sıfırlama bağlantısı, kimlik doğrulayıcı uygulamayla iki adımlı doğrulama (TOTP) ve tek kullanımlık kurtarma kodları.

## Ödeme ve defter bütünlüğü

- Ödeme yalnızca zincirde doğrulanan transferle onaylanır; reorg, eksik/fazla/geç ödeme ayrı durumlara düşer.
- Ödeme talepleri yalnızca imzayla sahipliği kanıtlanmış adreslere açılabilir; oluşturma idempotent'tir.
- Defter çift taraflıdır ve değiştirilemez; düzeltmeler ters kayıtla yapılır. Mutabakat dört bağımsız kaydı karşılaştırır.
- İade, tam transfer zincirde bulunmadan onaylanmaz; bir işlem tek bir iadeyi kapatabilir.

## Entegrasyon

- Webhook'lar HMAC-SHA256 ve zaman damgasıyla imzalanır; sırlar şifreli saklanır ve yalnızca bir kez gösterilir.
- SSRF koruması: Worker hedef adresi kendisi çözer, iç ağ, loopback, link-local ve bulut metadata adreslerine bağlanmaz.

## Platform

- Katı Content Security Policy (yalnızca aynı köken; satır içi betik/stil yok), güvenlik başlıkları, hassas API yanıtları önbelleğe alınmaz.
- Konteynerler root olmayan kullanıcıyla, salt okunur dosya sistemiyle çalışır; Kubernetes'te NetworkPolicy ve kısıtlı Pod Security.
- Ana ağda işlem gönderimi varsayılan olarak kapalıdır.

## İddia edilmeyenler

Bağımsız güvenlik denetimi yapılmamıştır. PCI DSS, SOC 2, ISO 27001 sertifikası, lisans, KYC/AML ya da yaptırım taraması yoktur. Gerçek varlıklarla kullanım öncesinde bağımsız güvenlik testi ve hukuki/düzenleyici değerlendirme gerekir.
