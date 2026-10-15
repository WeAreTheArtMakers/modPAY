# 10 dakikalık demo senaryosu

Satış görüşmesinde müşterinin kendi telefonuyla, canlı test ağlarında.

1. **Başlangıç (1 dk):** https://modpay.runasp.net → "Demoyu başlat". Kayıt; arayüz Türkçe açılır (TR/EN düğmesi).
2. **Cüzdan (1 dk):** Cüzdan oluştur → kurtarma ifadesi yedeği. Vurgu: anahtar cihazda üretilir ve şifrelenir; sunucuya hiç gitmez.
3. **Test tokenları (30 sn):** "Test tokenları" → Ethereum Sepolia → "Cüzdanıma gönder". Saniyeler içinde gas ve USDC.
4. **QR ile tahsilat (2 dk):** Kasa (POS) → 1,5 USDC → QR. İkinci telefonda "Öde" → QR okut → İmza Koruması ekranı → onay. Kasa ekranı yeşile döner. Vurgu: "ödendi" yalnızca zincir onayıyla.
5. **Muhasebe (1 dk):** Defter → çift taraflı kayıt, mutabakat sıfır fark, CSV.
6. **Ağlar arası (2 dk):** Avalanche'taki bir faturayı Sepolia'daki USDC ile öde ("Başka ağdaki USDC ile öde"). Vurgu: köprü havuzu yok, sarmalanmış token yok; basmayı modPAY tamamlar.
7. **Güvenlik (1 dk):** Benzer bir adrese gönderim dene → adres zehirleme uyarısı; İmza Koruması'nın imzalanan baytları ekranla karşılaştırdığını anlat.
8. **Kurumsal (1 dk):** Geliştiriciler → API anahtarı ve webhook; kurum adı/renk/logo yapılandırması; kendi Kubernetes/SQL Server kurulumunuz.

Kapanış: 30 günlük pilot önerisi (pilot-ve-kabul.md).
