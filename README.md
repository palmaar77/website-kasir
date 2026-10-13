# ⚡ Veloce POS — Modern Cloud Point of Sale Web App

Aplikasi **Web POS (Point of Sale)** modern dengan desain antarmuka enterprise-grade (terinspirasi dari Stripe Terminal, Shopify POS, dan Vercel) yang kini **telah terhubung penuh ke backend API dan database MySQL**.

---

## 🚀 Tech Stack

- **Framework**: [Next.js 16 (App Router)](https://nextjs.org/) dengan Turbopack
- **Language**: [TypeScript](https://www.typescriptlang.org/) (Strict typing)
- **Database**: [MySQL / MariaDB](https://mariadb.org/) (XAMPP / Laragon / Standalone)
- **ORM**: [Prisma ORM 6](https://www.prisma.io/)
- **Styling**: [Tailwind CSS v4](https://tailwindcss.com/) dengan custom theme tokens & dark mode
- **UI Components**: Desain bergaya [shadcn/ui](https://ui.shadcn.com/)
- **State Management**: [Zustand](https://github.com/pmndrs/zustand) (Optimistic UI terintegrasi dengan REST API)
- **Audio Synthesis**: Native HTML5 Web Audio API (efek nada scan barcode, chime bayar, pop keranjang)
- **Effects**: Canvas Confetti saat checkout sukses

---

## 🗄️ Database & Backend Architecture

Database MySQL: `veloce_pos` di `127.0.0.1:3306`

### Model Entitas (Prisma Schema):
- **`Product` (`products`)**: `id`, `name`, `sku`, `barcode`, `category`, `price`, `costPrice`, `stock`, `minStock`, `unit`, `image`, `isActive`, `createdAt`, `updatedAt`
- **`Customer` (`customers`)**: `id`, `name`, `phone`, `email`, `tier`, `points`, `totalOrders`, `totalSpend`, `joinedDate`
- **`Transaction` (`transactions`)**: `id`, `invoiceNumber`, `customerId`, `orderType`, `subtotal`, `tax`, `serviceCharge`, `discount`, `total`, `paymentMethod`, `cashTendered`, `cashChange`, `cashierName`, `status`, `notes`
- **`TransactionItem` (`transaction_items`)**: `id`, `transactionId`, `productId`, `productName`, `sku`, `price`, `quantity`, `subtotal`, `notes`
- **`StoreSetting` (`store_settings`)**: Profil outlet, tarif pajak PB1, service charge, format footer struk, sound toggle

---

## 🔌 API Route Handlers

| Method | Endpoint | Deskripsi |
|---|---|---|
| `GET` | `/api/products` | Mengambil seluruh katalog produk dari database MySQL |
| `POST` | `/api/products` | Menambahkan produk baru ke database MySQL |
| `PUT` | `/api/products/[id]` | Memperbarui data produk |
| `DELETE` | `/api/products/[id]` | Menghapus produk dari database |
| `PATCH` | `/api/products/[id]/stock` | Menyesuaikan jumlah stok produk (+/-) |
| `GET` | `/api/customers` | Mengambil seluruh data pelanggan & poin loyalty |
| `POST` | `/api/customers` | Mendaftarkan member baru ke database MySQL |
| `PUT` | `/api/customers/[id]` | Memperbarui data member |
| `DELETE` | `/api/customers/[id]` | Menghapus member |
| `GET` | `/api/transactions` | Mengambil riwayat seluruh transaksi & item |
| `POST` | `/api/transactions` | **Checkout Atomik**: Membuat invoice, mengurangi stok produk di MySQL, dan menambah poin loyalty pelanggan |
| `PATCH` | `/api/transactions/[id]/void` | **Void Transaksi**: Membatalkan invoice dan mengembalikan stok barang ke database |
| `GET` | `/api/settings` | Mengambil profil toko dan konfigurasi kasir |
| `PUT` | `/api/settings` | Menyimpan perubahan konfigurasi toko ke database |
| `POST` | `/api/reset` | Mereset seluruh data kembali ke kondisi demo awal |

---

## ⌨️ Shortcut Keyboard Kasir

- **`F2`**: Buka halaman Kasir POS langsung dari halaman mana saja.
- **`Ctrl + K` / `Cmd + K`**: Membuka Command Palette (pencarian cepat menu, produk, dan navigasi).
- **`Enter` di Barcode Input**: Memindai barcode produk langsung masuk ke keranjang belanja.
- **`Esc`**: Menutup modal dialog atau drawer yang sedang aktif.

---

## 💻 Menjalankan Project

1. **Pastikan MySQL Aktif** (misal via XAMPP Control Panel atau Laragon port 3306).
2. **Koneksi Database** (`.env`):
   ```env
   DATABASE_URL="mysql://root:@127.0.0.1:3306/veloce_pos"
   ```
3. **Migrasi Database & Seeding Data Awal**:
   ```bash
   npx prisma db push
   node prisma/seed.js
   ```
4. **Jalankan Aplikasi**:
   ```bash
   npm run dev
   ```
5. Buka browser pada alamat:
   ```
   http://localhost:3000
   ```
