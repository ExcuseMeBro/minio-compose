# minio-compose

MinIO instance deploy qilish uchun Docker Compose konfiguratsiyasi. Coolify tarmog'iga ulangan, avtomatik bucket yaratish bilan.

## Arxitektura

- **minio** — asosiy MinIO server (S3-compatible object storage)
- **minio-init** — bir martalik init konteyner: alias qo'yadi, bucket yaratadi, public access beradi

## Muhit o'zgaruvchilari

`.env` fayl yarating yoki Coolify orqali belgilang:

| O'zgaruvchi | Tavsif |
|---|---|
| `MINIO_ACCESS_KEY` | Root foydalanuvchi nomi (access key) |
| `MINIO_SECRET_KEY` | Root parol (secret key) |
| `MINIO_BUCKET_NAME` | Avtomatik yaratiladigan bucket nomi |

### Ilova tomonida (masalan Django/FastAPI)

| O'zgaruvchi | Misol | Tavsif |
|---|---|---|
| `KYC_MINIO_BUCKET_NAME` | `nova-kyc` | Bucket nomi |
| `KYC_MINIO_ENDPOINT` | `https://kyc-storage.novainvestment.uz` | MinIO public URL |
| `KYC_MINIO_ACCESS_KEY` | `your-kyc-access-key` | Access key |
| `KYC_MINIO_SECRET_KEY` | `your-kyc-secret-key` | Secret key |
| `KYC_MINIO_USE_SSL` | `True` | SSL yoqilganmi |

## Portlar

| Port | Xizmat |
|---|---|
| `9000` | S3 API endpoint |
| `9001` | MinIO Console (web UI) |

Portlar faqat `expose` orqali ochilgan — tashqaridan kirish uchun Coolify reverse proxy ishlatiladi.

## Ishlatish

```bash
cp .env.example .env
# .env ni tahrirlang

docker compose up -d
```

Init konteyner (`minio-init`) MinIO tayyor bo'lguncha kutadi (healthcheck orqali), keyin bucket yaratib chiqadi va o'z-o'zidan to'xtaydi.

## Tarmoq

`coolify` external tarmog'iga ulangan — bu tarmoq Coolify tomonidan boshqariladi va oldindan mavjud bo'lishi kerak.

## Ma'lumotlarni saqlash

MinIO data `minio_data` nomli named volume da saqlanadi — konteyner o'chirilganda yo'qolmaydi.
