# Panduan Install Laravel Breeze di Laravel 10

## Prasyarat
- PHP >= 8.1
- Composer
- Node.js & NPM
- Laravel 10 sudah terinstall

## Langkah 1: Install Laravel Breeze
```bash
composer require laravel/breeze:^1.19 --dev
```

## Langkah 2: Install Breeze Scaffolding
```bash
php artisan breeze:install
```

Setelah menjalankan perintah di atas, kamu akan diminta untuk memilih stack frontend:
- **blade** - Blade dengan Alpine.js
- **react** - React dengan Inertia.js
- **vue** - Vue.js dengan Inertia.js
- **api** - API only (untuk mobile app atau SPA terpisah)

Contoh untuk memilih stack Blade:
```bash
php artisan breeze:install blade
```

## Langkah 3: Install Dependencies NPM
```bash
npm install
```

## Langkah 4: Build Assets
```bash
npm run dev
```

Atau untuk production:
```bash
npm run build
```

## Langkah 5: Migrate Database
```bash
php artisan migrate
```

## Langkah 6: Start Development Server
```bash
php artisan serve
```

## Fitur yang Tersedia Setelah Install
- ✅ Authentication (Login/Register)
- ✅ Password Reset
- ✅ Email Verification
- ✅ Profile Management
- ✅ Dashboard
- ✅ Middleware Authentication

## Route yang Tersedia
- `/login` - Halaman login
- `/register` - Halaman registrasi
- `/dashboard` - Dashboard (perlu login)
- `/profile` - Halaman profile (perlu login)
- `/forgot-password` - Reset password
- `/verify-email` - Verifikasi email

## Troubleshooting

### Error: Package not compatible
Pastikan menggunakan versi Breeze yang kompatibel dengan Laravel 10:
```bash
composer require laravel/breeze:^1.19 --dev
```

### Error: Node modules tidak ditemukan
Jalankan ulang instalasi NPM:
```bash
rm -rf node_modules package-lock.json
npm install
```

### Error: Vite tidak bisa build
Pastikan file `vite.config.js` sudah benar dan jalankan:
```bash
npm run dev
```

## Catatan Penting
- Laravel Breeze versi 1.19 adalah versi yang kompatibel dengan Laravel 10
- Jangan lupa untuk menjalankan `npm run dev` setiap kali development
- Untuk production, gunakan `npm run build`
- Database harus sudah dikonfigurasi di file `.env` sebelum menjalankan migration
