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

## Teslimatlar

Kurulum belgeleri, kabul tutanağı, bulgular ve önerilen canlıya geçiş planı (güvenlik denetimi ve düzenleyici değerlendirme adımları dahil).
