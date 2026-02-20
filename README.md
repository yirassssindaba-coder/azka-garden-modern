<div align="center">

<!-- Animated Wave Header -->
<img src="https://capsule-render.vercel.app/api?type=waving&height=210&color=0:16a34a,100:22c55e&text=Azka%20Garden%20v2&fontSize=56&fontColor=ffffff&animation=fadeIn&fontAlignY=35&desc=Plant%20Store%20%7C%20QRIS%20Payments%20%2B%20Live%20Order%20Tracking&descAlignY=58" />

<!-- Typing SVG -->
<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=18&duration=3000&pause=700&color=16A34A&center=true&vCenter=true&width=780&lines=Modern+Plant+Store+E-commerce+for+Indonesia;QRIS+(DANA%2FOVO%2FShopeePay)%2C+Kartu+Visa%2FMastercard%2C+VA+BCA%2FMandiri;Webhook+auto-update+order+%E2%86%92+PAID+%2B+ledger+saldo;Live+location+tracking+pesanan+di+peta+(Mapbox)" />

<!-- Badges -->
<p>
  <img src="https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=white" />
  <img src="https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white" />
  <img src="https://img.shields.io/badge/Vite-6-646CFF?logo=vite&logoColor=white" />
  <img src="https://img.shields.io/badge/Tailwind-3-06B6D4?logo=tailwindcss&logoColor=white" />
  <img src="https://img.shields.io/badge/Express-API-111827?logo=express&logoColor=white" />
  <img src="https://img.shields.io/badge/Supabase-optional-3ECF8E?logo=supabase&logoColor=white" />
  <img src="https://img.shields.io/badge/Payments-Midtrans%20%7C%20Xendit-16a34a" />
  <img src="https://img.shields.io/badge/QRIS-DANA%20%7C%20OVO%20%7C%20Bank-16a34a" />
  <img src="https://img.shields.io/badge/Mapbox-Live%20Tracking-111827?logo=mapbox&logoColor=white" />
</p>

<p align="center">
  Website e-commerce tanaman hias: checkout otomatis (QRIS/e-wallet/kartu/VA) + tracking lokasi pesanan real-time.
</p>

</div>

---

## Table of Contents

- [Status Implementasi](#status-implementasi)
- [Fitur Utama](#fitur-utama)
- [Arsitektur & Data Flow](#arsitektur--data-flow)
- [Tech Stack](#tech-stack)
- [Struktur Project](#struktur-project)
- [Keamanan](#keamanan)
- [Installation & Setup](#installation--setup)
- [Setup Payment Gateway](#setup-payment-gateway)
- [Setup Tracking Kurir](#setup-tracking-kurir)
- [Deployment](#deployment)
- [Contributing](#contributing)

---

## Status Implementasi

| Domain | Status | Keterangan |
|---|---|---|
| ✅ Frontend E-commerce | **SELESAI** | Katalog, cart, checkout, order history (demo) |
| ✅ Checkout Otomatis | **SELESAI** | Midtrans Snap / Xendit Invoice |
| ✅ Metode Pembayaran | **SELESAI** | **QRIS** (DANA/OVO/dll via QR), **Kartu** (Visa/Mastercard), **VA** (BCA/Mandiri/dll)\* |
| ✅ Webhook Payment | **SELESAI** | Paid → order auto update + ledger saldo |
| ✅ Tracking Lokasi Pesanan | **SELESAI** | Peta Mapbox + timeline status + lokasi terakhir |
| ✅ Admin Portal (Demo) | **SELESAI** | Dashboard, Orders, Payments Ledger |
| ✅ Database Schema (Opsional) | **SELESAI** | Supabase: orders, tracking_events, ledger |

\* Ketersediaan metode bergantung aktivasi di akun payment gateway (merchant).

---

## Fitur Utama

<details>
<summary><b>🛒 E-commerce Complete (klik untuk buka/tutup)</b></summary>

```txt
✅ Katalog produk (demo data)
✅ Detail produk + rekomendasi
✅ Shopping cart (persist localStorage)
✅ Checkout flow: ongkir + total
✅ Order history (demo)
✅ Responsive + performance friendly
```
</details>

<details>
<summary><b>💳 Pembayaran QRIS / E-Wallet / Kartu / VA (klik untuk buka/tutup)</b></summary>

```txt
✅ Provider: Midtrans Snap (recommended) atau Xendit Invoice
✅ QRIS: scan dari app pembayaran (DANA/OVO/ShopeePay/LinkAja/dll tergantung provider)
✅ Kartu: Visa / Mastercard (secure 3DS bila diaktifkan)
✅ Virtual Account: BCA, Mandiri, BRI, dll (bergantung konfigurasi)
✅ Webhook: payment confirmed → order status otomatis menjadi PAID
✅ Ledger saldo internal untuk rekonsiliasi (admin)
```
</details>

<details>
<summary><b>📍 Live Order Location Tracking (klik untuk buka/tutup)</b></summary>

```txt
✅ Halaman Tracking: /tracking/:orderId
✅ Peta Mapbox + marker lokasi terakhir kurir
✅ Timeline status (confirmed → allocated → delivered)
✅ Admin bisa push update lokasi (lat/lng) untuk simulasi
✅ Siap dihubungkan ke provider kurir (mis. Biteship) via webhook
```
</details>

<details>
<summary><b>🧾 Ledger Saldo (klik untuk buka/tutup)</b></summary>

```txt
✅ Saat webhook PAID diterima → ledger credit otomatis
✅ Admin bisa lihat saldo total + histori transaksi
✅ Catatan: saldo ini adalah pencatatan internal (rekonsiliasi)
```
</details>

---

## Arsitektur & Data Flow

```mermaid
flowchart LR
U[Customer] -->|Browse/Cart/Checkout| FE[Frontend (React)]
A[Admin] -->|Orders/Tracking/Ledger| FE

FE -->|Create Order| API[Express API]
FE -->|Create Payment| API

API -->|Create Tx| PAY[(Midtrans / Xendit)]
PAY -->|Webhook: PAID| API

API -->|Upsert| DB[(Supabase Postgres)]
API -->|Insert tracking_events| DB
API -->|Insert ledger| DB
DB -->|Read tracking timeline| FE
```

---

## Tech Stack

- **Frontend:** React 18, TypeScript, Vite, Tailwind
- **Backend:** Node.js + Express
- **Maps:** Mapbox GL JS
- **Database (opsional):** Supabase (Postgres)
- **Payments:** Midtrans (Snap) / Xendit (Invoice)

---

## Struktur Project

```txt
azka-garden-modern/
├─ backend/
│  ├─ server.js                 # API: orders, payments, webhooks, tracking, ledger
│  └─ package.json
├─ supabase/
│  └─ schema.sql                # SQL tables: orders, tracking_events, ledger
├─ src/
│  ├─ pages/
│  │  ├─ admin/                 # Admin dashboard, orders, payments
│  │  └─ ...                    # Home, Products, Cart, Checkout, Tracking
│  ├─ lib/                      # api, cart, payments, tracking, storage
│  └─ data/                     # demo products
├─ .env.example
└─ package.json
```

---

## Keamanan

- **Midtrans:** verifikasi `signature_key` (SHA512) sebelum memproses webhook.
- **Xendit:** verifikasi header `x-callback-token` sebelum memproses webhook.
- **Supabase (opsional):** gunakan Service Role Key hanya di backend (jangan taruh di frontend).

> Untuk demo/dev, kamu bisa set `MIDTRANS_SKIP_SIGNATURE=true`. Jangan gunakan di production.

---

## Installation & Setup

### Prerequisites

```txt
Node.js 18+
npm
```

### Quick Start (PowerShell)

1) Setup env:

```powershell
cd azka-garden-modern
copy .env.example .env
# isi minimal: VITE_API_BASE_URL=http://localhost:8787
```

2) Run backend:

```powershell
npm install --prefix backend
node backend/server.js
```

3) Run frontend:

```powershell
npm install
npm run dev
```

- Frontend: `http://localhost:5173`
- Backend: `http://localhost:8787`

---

## Setup Payment Gateway

### Midtrans (Snap)

Isi `.env`:

```txt
MIDTRANS_SERVER_KEY=...
VITE_MIDTRANS_CLIENT_KEY=...
MIDTRANS_IS_PRODUCTION=false
```

Webhook URL (contoh):

```txt
https://YOUR_DOMAIN/api/webhooks/midtrans
```

### Xendit (Invoice)

Isi `.env`:

```txt
XENDIT_SECRET_KEY=...
XENDIT_WEBHOOK_TOKEN=...   # header x-callback-token
FRONTEND_BASE_URL=https://YOUR_FRONTEND_DOMAIN
```

Webhook URL (contoh):

```txt
https://YOUR_DOMAIN/api/webhooks/xendit
```

---

## Setup Tracking Kurir

Ada 2 mode:

1) **Manual (demo / driver app)**
- Buka Admin → Orders
- Push event `status + lat/lng`
- Tracking page otomatis menampilkan lokasi terakhir (polling tiap 8 detik)

2) **Integrasi kurir (production)**
- Terima webhook tracking provider kurir
- Map status kurir → `tracking_events`
- Frontend cukup baca `/api/tracking/:orderId`

---

## Deployment

- Frontend: Vercel / Netlify / Static Hosting
- Backend: Render / Railway / VPS
- Pastikan webhook URL **https** dan env sudah terisi.

---

## Contributing

```bash
1. Fork repository
2. git checkout -b feature/awesome
3. git commit -m "feat: awesome"
4. git push origin feature/awesome
5. Open Pull Request
```
