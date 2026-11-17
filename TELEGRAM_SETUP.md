# PGPM Mini App — Telegram-ready package v1.1

Pakej ini menambah sokongan Telegram Mini Apps SDK kepada sistem PGPM. Aplikasi tetap boleh dibuka sebagai website/PWA, tetapi apabila dibuka melalui Telegram ia memanggil `Telegram.WebApp.ready()`, mengembangkan paparan, menetapkan warna host dan menggunakan haptic feedback untuk hasil scan/check-in.

## A. Deploy website dahulu
1. Extract ZIP dan upload kandungan folder `PGPM_Mini_App` ke repository GitHub.
2. Deploy repository melalui Cloudflare Pages (Framework: None, Build command kosong, output directory `/`).
3. Pastikan URL akhir menggunakan HTTPS. Kamera barcode memerlukan HTTPS dan kebenaran kamera.
4. Konfigurasi Supabase seperti bahagian C di bawah.

## B. Daftarkan Telegram Mini App
1. Buka Telegram dan cari `@BotFather`.
2. Hantar `/newbot`, ikut arahan, tetapkan nama dan username bot. Simpan bot token secara sulit; JANGAN masukkan token ke dalam `assets/app.js`, GitHub atau frontend.
3. Dalam BotFather, gunakan `/setdomain`, pilih bot anda dan masukkan domain HTTPS Cloudflare Pages anda sahaja (contoh `nama-projek.pages.dev`, tanpa `https://` atau path).
4. Untuk butang menu, gunakan `/setmenubutton`, pilih bot, masukkan teks seperti `Buka PGPM`, kemudian masukkan URL HTTPS penuh aplikasi anda. Jika menu-button flow tidak ditawarkan pada akaun BotFather anda, gunakan `/mybots` → pilih bot → Bot Settings → Menu Button.
5. Buka bot tersebut, tekan butang menu `Buka PGPM`. Mini App akan berjalan di dalam Telegram.

## C. Supabase
1. Cipta projek di https://supabase.com.
2. SQL Editor → New query → jalankan keseluruhan `supabase/schema.sql`.
3. Project Settings → API: salin Project URL dan anon/publishable key.
4. Masukkan kedua-duanya dalam `CONFIG` di bahagian atas `assets/app.js`. Jangan masukkan `service_role` key.
5. Cipta pengguna melalui Supabase Authentication → Users. Masukkan profil pentadbir menggunakan UUID pengguna seperti arahan SQL dalam seksyen setup v1.0 (jadual `profiles`).
6. Cipta akaun ahli dan profil masing-masing. Uji akses menggunakan akaun admin dan member yang berasingan.

## D. Cara penggunaan
- Ahli log masuk menggunakan akaun emel/kata laluan Supabase yang telah didaftarkan.
- Ahli boleh paparkan barcode dan buat self check-in.
- Admin boleh guna kamera atau scanner USB/Bluetooth yang bertindak sebagai keyboard.
- Dashboard, profil, rekod gelanggang, yuran dan program menggunakan Supabase.

## E. Perkara keselamatan penting
- Integrasi Telegram dalam pakej ini ialah integrasi host UI/SDK sahaja. Log masuk masih menggunakan Supabase email/password. Ia TIDAK mendakwa mengesahkan identiti Telegram secara kriptografi atau memetakan akaun Telegram kepada akaun ahli secara automatik.
- Untuk Telegram single sign-on yang selamat, perlu tambah backend (contohnya Supabase Edge Function) yang mengesahkan `Telegram.WebApp.initData` menggunakan bot token yang disimpan sebagai secret di server. Jangan percaya `initDataUnsafe` untuk kebenaran/role.
- Jangan letak bot token atau Supabase service-role key dalam frontend.
- BotFather hanya menghubungkan URL aplikasi kepada bot; ia tidak host website atau menyediakan database.
- Pembayaran yuran masih rekod manual; tiada payment gateway. Eksport CSV boleh dibuka dengan Excel.

## Troubleshooting
- Bot buka browser luar: semak URL Menu Button dan `/setdomain`; cuba buka bot dalam aplikasi Telegram rasmi yang dikemas kini.
- Kamera tak berfungsi: guna HTTPS, beri permission kamera, atau gunakan scanner USB/Bluetooth.
- Data tak keluar: semak Supabase URL/key, SQL schema, akaun Auth dan RLS policies.

Versi: 1.1 — Telegram Mini Apps SDK enabled.
