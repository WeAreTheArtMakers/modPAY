# Webhook entegrasyonu / webhook integration

Uygulamada **Developers** sayfasından ya da API'den (`/api/webhooks`) uç nokta ekleyin. / Add endpoints on the **Developers** page or via the API.

## Olaylar / events

| Type | Ne zaman / when |
|---|---|
| `payment.created` | Ödeme talebi oluşturuldu / request created |
| `payment.status_changed` | Durum değişti (ör. Confirming → Confirmed, Expired) / status changed |
| `payment.updated` | Tutar/onay değişti / amounts or confirmations changed |
| `refund.created`, `refund.status_changed` | İade kaydedildi, yayınlandı, onaylandı ya da başarısız / refund recorded, broadcast, confirmed or failed |
| `webhook.test` | Panelden test / dashboard test |

Kesinleşme yalnızca `Confirmed` durumundadır. Tutarlar en küçük birim cinsinden tam sayı dizgesidir (`token.decimals` ile birlikte).

## İmza / signature

`modPAY-Signature: t=<unix>,v1=<hex>`; `v1 = HMAC-SHA256(secret, "<t>.<ham gövde / raw body>")`. Ham gövde üzerinde doğrulayın, sabit zamanlı karşılaştırın, eski zaman damgalarını reddedin; `modPAY-Event-Id` ile tekrarları ayıklayın (en az bir kez teslim).

```js
import { createHmac, timingSafeEqual } from 'node:crypto';
export function verify(rawBody, header, secret, toleranceSec = 300) {
  const p = Object.fromEntries(header.split(',').map((x) => x.split('=')));
  if (Math.abs(Date.now() / 1000 - Number(p.t)) > toleranceSec) return false;
  const expected = createHmac('sha256', secret).update(`${p.t}.${rawBody}`).digest('hex');
  return timingSafeEqual(Buffer.from(expected), Buffer.from(p.v1));
}
```

## Teslim / delivery

10 saniye içinde `2xx` dönün. Aksi halde 10 sn, 1 dk, 5 dk, 30 dk, 2 sa, 6 sa, 12 sa sonra yeniden denenir (8 deneme); panelden elle yeniden gönderilebilir. Art arda 50 başarısızlıkta uç nokta devre dışı kalır.
Respond `2xx` within 10 seconds; otherwise retries follow the schedule above (8 attempts) and can be triggered manually. 50 consecutive failures disable the endpoint.
