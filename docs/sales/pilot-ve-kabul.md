# Pilot ve kabul testi

## Pilot kapsamı (30 gün, testnet)

1. **Başlangıç (1. gün):** hedefler, ağ ve token seçimi (ör. Sepolia USDC), webhook uç noktanız.
2. **Kurulum (1–3. gün):** barındırılan ortam ya da kendi Kubernetes kümenize Helm kurulumu.
3. **Entegrasyon (1–2. hafta):** webhook imza doğrulaması, ERP/muhasebe eşleştirmesi, ekip rolleri.
4. **Kabul (3–4. hafta):** aşağıdaki senaryoların sizin ekibinizle birlikte koşulması; bulgular raporu ve canlıya geçiş önerisi.

## Kabul senaryoları

| # | Senaryo | Beklenen |
|---|---|---|
| 1 | Kayıt, e-posta doğrulama, iki adımlı doğrulama | Girişte TOTP istenir; kurtarma kodu bir kez çalışır |
| 2 | Cüzdan oluşturma ve yedek doğrulaması | Kurtarma ifadesi hiçbir ağ isteğinde yer almaz |
| 3 | Adres sahipliği doğrulama | İmza sunucuda doğrulanır; tek kullanımlık |
| 4 | Fatura + QR, modPAY ile ödeme | Onay derinliğinde *Onaylandı*; Dr 1010 / Cr 4010 |
| 5 | Aynı fatura, MetaMask ile ödeme (EIP-681) | Hash bildirmeden algılanır ve onaylanır |
| 6 | Kısmi ödeme, fazla ödeme, süresi geçmiş ödeme | Ayrı durumlar; geç ödeme incelemeye düşer |
| 7 | İade | Zincirde tam transfer bulununca onaylanır; Dr 4090 / Cr 1010 |
| 8 | Mutabakat | Sıfır uyumsuzluk; borç = alacak |
| 9 | Webhook | İmza doğrulanır; hatada yeniden deneme; teslimat günlüğü |
| 10 | Organizasyon rolleri | İzleyici talep oluşturamaz; üye olmayan 404 alır |
| 11 | Test tokenları | Yeni kullanıcı tek tıkla gas ve USDC alır; aynı gün ikinci talep reddedilir |
| 12 | Başka ağdaki USDC ile fatura | Sepolia'da yakılan USDC Fuji'deki faturayı kapatır; basmayı modPAY yapar; fatura *Onaylandı* |
| 14 | Dört göz onayı | Oluşturan kendi ödeme çıkışını onaylayamaz; ikinci kişi onaylar; zincirde doğrulanınca Dr 5010 / Cr 1010 |
| 15 | Kural gevşetme | Limit artışı 24 saat sonra yürürlüğe girer; bu sürede iptal edilebilir |
| 16 | Yaptırım taraması | OFAC listesindeki adrese ödeme çıkışı ve iade reddedilir; böyle bir ödeyenden gelen ödeme incelemeye düşer |
| 17 | Travel Rule | Eşik üstü ödeme alıcı bilgisi olmadan oluşturulamaz; IVMS101 imzalı webhook ile iletilir |
| 18 | Kurumsal giriş | Kurumun kimlik sağlayıcısıyla (OIDC) giriş; iki adımlı doğrulama zorunluluğu |
| 19 | ERP | SAP/Logo/Mikro dosyası hesap eşlemesiyle üretilir ve muhasebe danışmanınca içe aktarılır |
| 13 | Kurumsal kimlik ve dil | Kurum adı/renk/logo yapılandırmadan gelir; arayüz Türkçe ve İngilizce |

## Teslimatlar

Kurulum belgeleri, kabul tutanağı, bulgular ve önerilen canlıya geçiş planı (güvenlik denetimi ve düzenleyici değerlendirme adımları dahil).
