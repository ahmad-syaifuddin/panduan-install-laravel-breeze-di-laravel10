# Laravel 10 + Breeze 1.19 Installation Guide

Panduan lengkap instalasi Laravel 10 dengan Breeze 1.19, tersedia dalam **2 pilihan** styling: Tailwind CSS v3 (default) atau upgrade ke Tailwind CSS v4 (modern).

## 📋 Pilihan Styling

| Pilihan | Deskripsi | Kelebihan | Kekurangan |
|---------|-----------|-----------|------------|
| **🔷 Tailwind v3** | Setup default Breeze 1.19 | ✅ Stabil & teruji<br>✅ Dokumentasi lengkap<br>✅ Quick setup | ❌ Design lama<br>❌ No dark mode default |
| **🔥 Tailwind v4** | Upgrade modern dengan UI terbaru | ✅ Design modern<br>✅ Dark mode built-in<br>✅ Better performance<br>✅ Enhanced UX | ❌ Setup lebih kompleks<br>❌ Perlu migrasi manual |

---

# 🔷 Pilihan 1: Laravel 10 + Breeze 1.19 + Tailwind v3 (Default)

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

## Fitur yang Tersedia (Tailwind v3)
- ✅ Authentication (Login/Register)
- ✅ Password Reset
- ✅ Email Verification
- ✅ Profile Management
- ✅ Dashboard
- ✅ Middleware Authentication
- ✅ Responsive design
- ✅ Form validation

## Route yang Tersedia
- `/login` - Halaman login
- `/register` - Halaman registrasi
- `/dashboard` - Dashboard (perlu login)
- `/profile` - Halaman profile (perlu login)
- `/forgot-password` - Reset password
- `/verify-email` - Verifikasi email

---

# 🔥 Pilihan 2: Laravel 10 + Breeze 1.19 + Tailwind v4 (Modern Upgrade)

## Prerequisites (sama seperti Pilihan 1)
- PHP >= 8.1
- Composer
- Node.js & NPM
- Laravel 10 sudah terinstall

## Step 1: Install Laravel Breeze (sama seperti Pilihan 1)
```bash
composer require laravel/breeze:^1.19 --dev
php artisan breeze:install blade
```

## Step 2: Remove Tailwind v3 & Install v4

### Remove Tailwind v3 Dependencies
```bash
# Remove Tailwind v3 packages
npm uninstall tailwindcss postcss autoprefixer @tailwindcss/forms

# Remove config files
rm -f tailwind.config.js postcss.config.js
```

### Install Tailwind CSS v4
```bash
# Install Tailwind v4 and FontAwesome
npm install @tailwindcss/vite @fortawesome/fontawesome-free
```

## Step 3: Configure Vite for Tailwind v4

Update `vite.config.js`:

```javascript
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';
import tailwindcss from '@tailwindcss/vite';

export default defineConfig({
    plugins: [
        laravel({
            input: ['resources/css/app.css', 'resources/js/app.js'],
            refresh: true,
        }),
        tailwindcss(),
    ],
});
```

## Step 4: Update CSS Configuration

Replace the content of `resources/css/app.css`:

```css
@import "tailwindcss";
@import "@fortawesome/fontawesome-free/css/all.min.css";

@custom-variant dark (&:where(.dark, .dark *));

@source "resources/views/**/*.blade.php";

@theme {
  --color-primary: #3b82f6;
  --color-primary-50: #eff6ff;
  --color-primary-100: #dbeafe;
  --color-primary-500: #3b82f6;
  --color-primary-600: #2563eb;
  --color-primary-700: #1d4ed8;
  --color-primary-900: #1e3a8a;
  
  --color-success: #10b981;
  --color-warning: #f59e0b;
  --color-error: #ef4444;
  --color-info: #3b82f6;
}

@layer base {
  html {
    @apply h-full;
  }
  
  body {
    @apply h-full bg-gray-50 dark:bg-gray-900 text-gray-900 dark:text-gray-100;
  }
}

@layer components {
  .btn {
    @apply px-4 py-2 rounded-lg font-medium transition-colors duration-200;
  }
  
  .btn-primary {
    @apply bg-primary-500 text-white hover:bg-primary-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-primary-500;
  }
  
  .btn-secondary {
    @apply bg-gray-200 text-gray-900 hover:bg-gray-300 dark:bg-gray-700 dark:text-gray-100 dark:hover:bg-gray-600;
  }
  
  .form-input {
    @apply block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-primary-500 focus:border-primary-500 dark:bg-gray-800 dark:border-gray-600 dark:text-white dark:placeholder-gray-400;
  }
  
  .form-label {
    @apply block text-sm font-medium text-gray-700 dark:text-gray-300;
  }
  
  .form-error {
    @apply mt-1 text-sm text-red-600 dark:text-red-400;
  }
}
```

## Step 5: Update Breeze Views for Tailwind v4

### Guest Layout (`resources/views/layouts/guest.blade.php`)

```php
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}" class="{{ request()->cookie('theme', 'light') }}">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <meta name="csrf-token" content="{{ csrf_token() }}">

        <title>{{ config('app.name', 'Laravel') }}</title>

        <!-- Fonts -->
        <link rel="preconnect" href="https://fonts.bunny.net">
        <link href="https://fonts.bunny.net/css?family=figtree:400,500,600&display=swap" rel="stylesheet" />

        <!-- Scripts -->
        @vite(['resources/css/app.css', 'resources/js/app.js'])
    </head>
    <body class="font-sans text-gray-900 dark:text-gray-100 antialiased">
        <div class="min-h-screen flex flex-col sm:justify-center items-center pt-6 sm:pt-0 bg-gray-50 dark:bg-gray-900">
            <!-- Dark Mode Toggle -->
            <div class="absolute top-4 right-4">
                <button onclick="toggleDarkMode()" class="p-2 rounded-lg bg-white dark:bg-gray-800 shadow-md border border-gray-200 dark:border-gray-700 text-gray-500 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-200 transition-colors">
                    <i class="fas fa-moon w-5 h-5 dark:hidden"></i>
                    <i class="fas fa-sun w-5 h-5 hidden dark:block"></i>
                </button>
            </div>

            <div class="w-full sm:max-w-md mt-6 px-6 py-4 bg-white dark:bg-gray-800 shadow-lg overflow-hidden sm:rounded-lg border border-gray-200 dark:border-gray-700">
                <!-- Logo -->
                <div class="flex justify-center mb-6">
                    <a href="/">
                        <div class="w-20 h-20 bg-primary-500 rounded-full flex items-center justify-center">
                            <i class="fas fa-user-shield text-white text-2xl"></i>
                        </div>
                    </a>
                </div>

                {{ $slot }}
            </div>
        </div>

        <script>
            function toggleDarkMode() {
                const html = document.documentElement;
                const isDark = html.classList.contains('dark');
                
                if (isDark) {
                    html.classList.remove('dark');
                    document.cookie = 'theme=light; path=/; max-age=31536000';
                } else {
                    html.classList.add('dark');
                    document.cookie = 'theme=dark; path=/; max-age=31536000';
                }
            }
        </script>
    </body>
</html>
```

### Login View (`resources/views/auth/login.blade.php`)

```php
<x-guest-layout>
    <!-- Session Status -->
    <x-auth-session-status class="mb-4" :status="session('status')" />

    <div class="mb-6 text-center">
        <h2 class="text-2xl font-bold text-gray-900 dark:text-white">Welcome Back</h2>
        <p class="text-gray-600 dark:text-gray-400 mt-2">Sign in to your account</p>
    </div>

    <form method="POST" action="{{ route('login') }}">
        @csrf

        <!-- Email Address -->
        <div class="mb-4">
            <x-input-label for="email" class="dark:text-gray-400" :value="__('Email')" />
            <x-text-input id="email" class="form-input mt-1" type="email" name="email" :value="old('email')" required autofocus autocomplete="username" />
            <x-input-error :messages="$errors->get('email')" class="form-error" />
        </div>

        <!-- Password -->
        <div class="mb-4">
            <x-input-label for="password" class="dark:text-gray-400" :value="__('Password')" />
            <x-text-input id="password" class="form-input mt-1" type="password" name="password" required autocomplete="current-password" />
            <x-input-error :messages="$errors->get('password')" class="form-error" />
        </div>

        <!-- Remember Me -->
        <div class="block mb-4">
            <label for="remember_me" class="inline-flex items-center">
                <input id="remember_me" type="checkbox" class="rounded border-gray-300 text-primary-600 shadow-sm focus:ring-primary-500 dark:border-gray-600 dark:bg-gray-800" name="remember">
                <span class="ml-2 text-sm text-gray-600 dark:text-gray-400">{{ __('Remember me') }}</span>
            </label>
        </div>

        <div class="flex items-center justify-between">
            @if (Route::has('password.request'))
                <a class="text-sm text-primary-600 hover:text-primary-500 dark:text-primary-400 dark:hover:text-primary-300" href="{{ route('password.request') }}">
                    {{ __('Forgot your password?') }}
                </a>
            @endif

            <x-primary-button class="btn-primary ml-auto">
                {{ __('Log in') }}
            </x-primary-button>
        </div>

        @if (Route::has('register'))
            <div class="mt-6 text-center">
                <p class="text-sm text-gray-600 dark:text-gray-400">
                    Don't have an account?
                    <a href="{{ route('register') }}" class="text-primary-600 hover:text-primary-500 dark:text-primary-400 dark:hover:text-primary-300 font-medium">
                        Sign up here
                    </a>
                </p>
            </div>
        @endif
    </form>
</x-guest-layout>
```

### Register View (`resources/views/auth/register.blade.php`)

```php
<x-guest-layout>
    <div class="mb-6 text-center">
        <h2 class="text-2xl font-bold text-gray-900 dark:text-white">Create Account</h2>
        <p class="text-gray-600 dark:text-gray-400 mt-2">Join us today</p>
    </div>

    <form method="POST" action="{{ route('register') }}">
        @csrf

        <!-- Name -->
        <div class="mb-4">
            <x-input-label for="name" class="dark:text-gray-400" :value="__('Name')" />
            <x-text-input id="name" class="form-input mt-1" type="text" name="name" :value="old('name')" required autofocus autocomplete="name" />
            <x-input-error :messages="$errors->get('name')" class="form-error" />
        </div>

        <!-- Email Address -->
        <div class="mb-4">
            <x-input-label for="email" class="dark:text-gray-400" :value="__('Email')" />
            <x-text-input id="email" class="form-input mt-1" type="email" name="email" :value="old('email')" required autocomplete="username" />
            <x-input-error :messages="$errors->get('email')" class="form-error" />
        </div>

        <!-- Password -->
        <div class="mb-4">
            <x-input-label for="password" class="dark:text-gray-400" :value="__('Password')" />
            <x-text-input id="password" class="form-input mt-1" type="password" name="password" required autocomplete="new-password" />
            <x-input-error :messages="$errors->get('password')" class="form-error" />
        </div>

        <!-- Confirm Password -->
        <div class="mb-6">
            <x-input-label for="password_confirmation" class="dark:text-gray-400" :value="__('Confirm Password')" />
            <x-text-input id="password_confirmation" class="form-input mt-1" type="password" name="password_confirmation" required autocomplete="new-password" />
            <x-input-error :messages="$errors->get('password_confirmation')" class="form-error" />
        </div>

        <div class="flex items-center justify-between">
            <a class="text-sm text-primary-600 hover:text-primary-500 dark:text-primary-400 dark:hover:text-primary-300" href="{{ route('login') }}">
                {{ __('Already registered?') }}
            </a>

            <x-primary-button class="btn-primary ml-auto">
                {{ __('Register') }}
            </x-primary-button>
        </div>
    </form>
</x-guest-layout>
```

### Dashboard View (`resources/views/dashboard.blade.php`)

```php
<x-app-layout>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 dark:text-gray-200 leading-tight">
            {{ __('Dashboard') }}
        </h2>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <!-- Welcome Card -->
            <div class="bg-white dark:bg-gray-800 overflow-hidden shadow-sm sm:rounded-lg mb-6">
                <div class="p-6 text-gray-900 dark:text-gray-100">
                    <div class="flex items-center space-x-4">
                        <div class="w-12 h-12 bg-primary-500 rounded-full flex items-center justify-center">
                            <i class="fas fa-user text-white"></i>
                        </div>
                        <div>
                            <h3 class="text-xl font-semibold">Welcome back, {{ Auth::user()->name }}! 👋</h3>
                            <p class="text-gray-600 dark:text-gray-400">You're logged in and ready to go.</p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Stats Cards -->
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6">
                <div class="bg-white dark:bg-gray-800 overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 text-center">
                        <div class="text-3xl font-bold text-primary-600 dark:text-primary-400">24</div>
                        <div class="text-sm text-gray-600 dark:text-gray-400 mt-1">Total Projects</div>
                    </div>
                </div>
                
                <div class="bg-white dark:bg-gray-800 overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 text-center">
                        <div class="text-3xl font-bold text-green-600 dark:text-green-400">18</div>
                        <div class="text-sm text-gray-600 dark:text-gray-400 mt-1">Completed</div>
                    </div>
                </div>
                
                <div class="bg-white dark:bg-gray-800 overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 text-center">
                        <div class="text-3xl font-bold text-yellow-600 dark:text-yellow-400">6</div>
                        <div class="text-sm text-gray-600 dark:text-gray-400 mt-1">In Progress</div>
                    </div>
                </div>
            </div>

            <!-- Recent Activity -->
            <div class="bg-white dark:bg-gray-800 overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6">
                    <h3 class="text-lg font-semibold text-gray-900 dark:text-gray-100 mb-4">Recent Activity</h3>
                    <div class="space-y-4">
                        <div class="flex items-center space-x-3">
                            <div class="w-8 h-8 bg-green-100 dark:bg-green-900 rounded-full flex items-center justify-center">
                                <i class="fas fa-check text-green-600 dark:text-green-400 text-sm"></i>
                            </div>
                            <div class="flex-1">
                                <p class="text-sm text-gray-900 dark:text-gray-100">Project "Website Redesign" completed</p>
                                <p class="text-xs text-gray-500 dark:text-gray-400">2 hours ago</p>
                            </div>
                        </div>
                        
                        <div class="flex items-center space-x-3">
                            <div class="w-8 h-8 bg-blue-100 dark:bg-blue-900 rounded-full flex items-center justify-center">
                                <i class="fas fa-plus text-blue-600 dark:text-blue-400 text-sm"></i>
                            </div>
                            <div class="flex-1">
                                <p class="text-sm text-gray-900 dark:text-gray-100">New project "Mobile App" created</p>
                                <p class="text-xs text-gray-500 dark:text-gray-400">1 day ago</p>
                            </div>
                        </div>
                        
                        <div class="flex items-center space-x-3">
                            <div class="w-8 h-8 bg-purple-100 dark:bg-purple-900 rounded-full flex items-center justify-center">
                                <i class="fas fa-edit text-purple-600 dark:text-purple-400 text-sm"></i>
                            </div>
                            <div class="flex-1">
                                <p class="text-sm text-gray-900 dark:text-gray-100">Profile updated</p>
                                <p class="text-xs text-gray-500 dark:text-gray-400">3 days ago</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```

### Update Components

#### Input Label Component (`resources/views/components/input-label.blade.php`)
```php
@props(['value'])

<label {{ $attributes->merge(['class' => 'form-label']) }}>
    {{ $value ?? $slot }}
</label>
```

#### Text Input Component (`resources/views/components/text-input.blade.php`)
```php
@props(['disabled' => false])

<input {{ $disabled ? 'disabled' : '' }} {!! $attributes->merge(['class' => 'form-input']) !!}>
```

#### Primary Button Component (`resources/views/components/primary-button.blade.php`)
```php
<button {{ $attributes->merge(['type' => 'submit', 'class' => 'btn btn-primary']) }}>
    {{ $slot }}
</button>
```

#### Input Error Component (`resources/views/components/input-error.blade.php`)
```php
@props(['messages'])

@if ($messages)
    <ul {{ $attributes->merge(['class' => 'form-error']) }}>
        @foreach ((array) $messages as $message)
            <li>{{ $message }}</li>
        @endforeach
    </ul>
@endif
```

## Step 6: Build and Test

```bash
# Install dependencies and build
npm install
npm run build

# Migrate database
php artisan migrate

# Start development server
php artisan serve
```

## Fitur Tambahan (Tailwind v4)
- ✅ **Dark mode toggle** dengan persistent theme
- ✅ **Modern UI design** dengan better visual hierarchy
- ✅ **Enhanced animations** dan transitions
- ✅ **FontAwesome icons** terintegrasi
- ✅ **Better form validation styling**
- ✅ **Improved dashboard** dengan stats cards
- ✅ **Responsive design** yang lebih baik

---

# 🔧 Perbandingan Hasil

## Tailwind v3 (Default Breeze)
```bash
# Login page - basic styling
- Simple form layout
- Basic validation styling
- No dark mode
- Standard Breeze UI
```

## Tailwind v4 (Modern Upgrade)
```bash
# Login page - enhanced styling
- Modern card-based layout
- Dark mode toggle button
- Welcome messages
- Better visual feedback
- FontAwesome icons
- Enhanced UX flows
```

## Screenshot Perbandingan

### Tailwind v3 (Default)
- ✅ Simple dan functional
- ✅ Quick setup
- ❌ Basic styling
- ❌ No dark mode

### Tailwind v4 (Upgraded)
- ✅ Modern design
- ✅ Dark mode support
- ✅ Better UX
- ✅ Enhanced dashboard
- ❌ Setup lebih kompleks

---

# 🛠️ Troubleshooting

## Issues untuk Kedua Pilihan

### Error: Package not compatible
```bash
# Pastikan menggunakan versi yang benar
composer require laravel/breeze:^1.19 --dev
```

### Error: Node modules tidak ditemukan
```bash
rm -rf node_modules package-lock.json
npm install
```

### Error: Vite tidak bisa build
```bash
npm run dev
# atau
npm run build
```

## Issues Khusus Tailwind v4

### Styles tidak apply setelah migrasi
```bash
# Clear semua cache
npm run build
php artisan view:clear
php artisan config:clear
php artisan route:clear
```

### Dark mode tidak bekerja
```bash
# Pastikan JavaScript berjalan
# Check browser console untuk errors
# Clear browser cookies
```

### FontAwesome icons tidak muncul
```bash
# Reinstall FontAwesome
npm install @fortawesome/fontawesome-free
npm run build
```

---

# 📊 Rekomendasi Pilihan

## Pilih Tailwind v3 Jika:
- ✅ **Butuh setup cepat** untuk development
- ✅ **Tim belum familiar** dengan Tailwind v4
- ✅ **Project deadline ketat**
- ✅ **Tidak butuh dark mode** sekarang
- ✅ **Stable production** environment diprioritaskan

## Pilih Tailwind v4 Jika:
- ✅ **Ingin UI modern** dan professional
- ✅ **Dark mode** adalah requirement
- ✅ **Better user experience** diprioritaskan
- ✅ **Tim comfortable** dengan custom styling
- ✅ **Long-term project** dengan maintenance

---

# 🚀 Next Steps

## Setelah Setup Berhasil

### 1. Customize Branding
```bash
# Update logo, colors, fonts
# Modify theme variables
# Add company branding
```

### 2. Add More Features
```bash
# User roles & permissions
# Admin panel
# API endpoints
# Email templates
```

### 3. Production Deployment
```bash
# Environment setup
# Security hardening
# Performance optimization
# Monitoring setup
```

## 📚 Additional Resources

### Extending the Authentication System

**1. Add Social Login:**
```bash
composer require laravel/socialite
```

**2. Add Two-Factor Authentication:**
```bash
composer require pragmarx/google2fa-laravel
```

**3. Add Custom User Roles:**
```php
// Add to User model
public function hasRole($role)
{
    return $this->role === $role;
}
```

## 📚 Resources

- [Laravel Breeze Documentation](https://laravel.com/docs/10.x/starter-kits#laravel-breeze)
- [Tailwind CSS v3 Documentation](https://tailwindcss.com/docs)
- [Tailwind CSS v4 Documentation](https://tailwindcss.com/docs/v4-beta)
- [Laravel 10 Documentation](https://laravel.com/docs/10.x)

---

**Pilih sesuai kebutuhan project Anda! Kedua pilihan sama-sama valid dan functional.** 🎯
