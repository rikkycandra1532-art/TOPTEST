# TopMarket 🛒

Website marketplace lokal untuk kebutuhan harian — modern, ringan, dan cepat.
Dibangun murni dengan **HTML, CSS, dan JavaScript** (tanpa framework, tanpa build tool),
supaya mudah dipelajari, di-hosting, dan dikembangkan siapa saja.

## Cara Menjalankan

Tidak perlu instalasi apa pun.

1. Ekstrak folder ini.
2. Buka file `index.html` langsung di browser, **atau**
3. Jalankan local server sederhana (disarankan, supaya semua fitur berjalan optimal):
   ```bash
   # Python
   python3 -m http.server 8000

   # atau Node.js
   npx serve .
   ```
   Lalu buka `http://localhost:8000`.

Untuk deploy, tinggal upload seluruh folder ke hosting statis mana pun
(Netlify, Vercel, GitHub Pages, cPanel, dsb) — tidak butuh server backend.

## Struktur Folder

```
topmarket/
├── index.html            → Home
├── produk.html            → Semua Produk (filter, cari, urutkan)
├── produk-detail.html     → Detail Produk (?id=...)
├── keranjang.html         → Keranjang (Cart)
├── checkout.html          → Checkout
├── tentang.html           → Tentang Kami
├── kontak.html             → Kontak
├── css/
│   └── style.css          → Semua styling (design token + komponen + responsive)
└── js/
    ├── data.js            → Data produk & kategori (sumber data)
    ├── cart.js            → Logika keranjang (tambah/hapus/qty, badge, toast)
    └── app.js             → Render header/footer, render tiap halaman, interaksi UI
```

## Cara Kerja Singkat

- **Tanpa backend.** Data produk berasal dari `js/data.js` (array statis). Untuk
  menyambungkan ke API/database sungguhan, cukup ganti isi `PRODUCTS` di file
  tersebut dengan hasil fetch dari server Anda.
- **Keranjang** disimpan di `localStorage` browser (key `topmarket_cart`), sehingga
  isi keranjang tetap ada walau halaman di-refresh atau ditutup. Menambah/mengubah
  item tidak pernah me-reload halaman — semua lewat JavaScript.
- **Header & footer** dirender sekali lewat `headerHTML()` / `footerHTML()` di
  `app.js` lalu disuntikkan ke `#headerPlaceholder` / `#footerPlaceholder` di
  setiap halaman. Ini artinya kalau Anda ingin mengubah menu atau footer,
  **cukup edit di satu tempat** (`app.js`), tidak perlu mengubah 7 file HTML satu-satu.
- **Routing** antar "page" memakai file HTML terpisah (multi-page), bukan SPA —
  ini sengaja dipilih supaya setiap halaman punya URL sendiri (baik untuk SEO),
  loading tetap ringan, dan strukturnya gampang dipahami pemula.

## Menambah Produk Baru

Buka `js/data.js`, tambahkan objek baru ke array `PRODUCTS`, contoh:

```js
{ id: 31, name: "Roti Tawar 400g", category: "sembako", price: 18000,
  discount: 0, rating: 4.5, stock: 40, sold: 60, isNew: true,
  image: img("Roti Tawar") }
```

Produk otomatis muncul di halaman **Semua Produk**, dan di **Home** jika masuk
kategori "terbaru" / "terlaris" / "promo" sesuai urutan sort di `app.js`.

## Mengganti Warna / Font

Semua warna & radius ada sebagai CSS variable di bagian atas `css/style.css`
(`:root { ... }`). Ubah nilai di sana untuk mengubah tema di seluruh halaman
sekaligus.

## Catatan Produksi

- Gambar produk memakai placeholder dari `placehold.co` — ganti `image:` di
  `data.js` dengan URL foto produk asli (disarankan format `.webp`, rasio 1:1)
  saat siap produksi.
- Form checkout & kontak saat ini mensimulasikan pengiriman data (tanpa server).
  Untuk produksi, sambungkan `fetch()` ke endpoint API/WhatsApp Business/email
  Anda di bagian `submit` masing-masing form (`js/app.js`).
- Peta di halaman Kontak memakai Google Maps embed tanpa API key — cukup ganti
  parameter `q=` pada `src` iframe dengan alamat toko Anda.
