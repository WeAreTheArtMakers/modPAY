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

Uyum durumu: güvenlik kontrolleri SOC 2 ve ISO/IEC 27001:2022 ile eşlenmiştir (hazırlık; sertifika bağımsız denetçi tarafından verilir). Kart verisi işlenmediği için PCI DSS kapsamı dışındadır. Lisans, KYC/AML ve yaptırım taraması kapsamı müşterinin iş modeline göre hukuki değerlendirmeyle belirlenir. Gerçek varlıklarla kullanım öncesinde bağımsız güvenlik testi ve hukuki/düzenleyici değerlendirme gerekir.

## İmza Koruması (ne görürseniz onu imzalarsınız)

Büyük kayıpların bir kısmı anahtar çalınmadan, imzacılara gösterilen işlem ile imzalanan işlemin farklı olmasıyla yaşandı. modPAY bu yüzden:

- İmzadan sonra, ağa göndermeden önce imzalı işlemi cihazda çözer, imzacıyı geri hesaplar ve ağ, nonce, gas, ücret, alıcı, tutar ve token kontratını kullanıcının onayladığı değerlerle tek tek karşılaştırır. Tek bir fark varsa işlem gönderilmez.
- Yalnızca iki tür işlem imzalar: düz ETH transferi ve ERC-20 `transfer(alıcı, tutar)`; sonuna eklenmiş bayt, token onayı (`approve`), `setApprovalForAll`, başka kontrat çağrısı, erişim listesi ya da yetki devri (EIP-7702) imzalanmaz.
- Alıcıyı değerlendirir: sahipliği imzayla kanıtlanmış satıcı ve iadede asıl ödeyen "doğrulanmış" gösterilir; kendi hesaplarınız ve daha önce gönderdiğiniz adresler ek adım istemez; yeni alıcıda adresin son 4, geçmişteki bir adresle aynı başlayıp biten (adres zehirleme) adreste son 8 karakter yazılmadan onay açılmaz; sıfır adres ve token kontratları reddedilir.

Sınır: İmza Koruması uygulamanın kendi kodunda çalışır. Sunulan uygulama kodunun kendisi ele geçirilirse (tedarik zinciri saldırısı) koruma da etkisizleştirilebilir; buna karşı katı CSP, değişmez sürümler ve bağımsız denetim gerekir. Donanım cüzdanın kendi ekranında doğrulamanın yerini tutmaz.
