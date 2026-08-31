# YTRVX

[![Build Status](https://github.com/nauraafii/ytrvx-module/actions/workflows/build.yml/badge.svg)](https://github.com/nauraafii/ytrvx-module/actions)
[![Latest Release](https://img.shields.io/github/v/release/nauraafii/ytrvx-module?label=Latest%20Release&color=blue)](https://github.com/nauraafii/ytrvx-module/releases/latest)

YTRVX adalah fork pribadi untuk membangun YouTube dan YouTube Music yang sudah dipatch. Repository ini membuat APK non-root serta modul ZIP untuk Magisk dan KernelSU dengan Morphe Patches.

## Unduh

Ambil file terbaru di [Releases](https://github.com/nauraafii/ytrvx-module/releases/latest).

- **APK**: untuk perangkat tanpa root. Pasang seperti APK biasa. Login Google memerlukan GmsCore yang kompatibel.
- **ZIP Magisk/KernelSU**: untuk perangkat root. Pasang melalui aplikasi root manager yang digunakan.
- Pilih file sesuai arsitektur perangkat. File dengan nama `all` mendukung beberapa arsitektur.
- Rilis baru menyertakan `SHA256SUMS.txt`. Unduh bersama file pilihan Anda, lalu jalankan `sha256sum -c SHA256SUMS.txt` sebelum memasang file.

## Build sendiri

1. Ubah [`config.toml`](config.toml) sesuai kebutuhan.
2. Buka [GitHub Actions](https://github.com/nauraafii/ytrvx-module/actions/workflows/build.yml).
3. Pilih **Run workflow** pada branch `main`.
4. Setelah selesai, ambil hasilnya dari halaman Releases.

Untuk build lokal, gunakan Java 21 atau lebih baru dan siapkan keystore milik sendiri melalui variabel `YTRVX_KEYSTORE_PATH` dan `YTRVX_KEYSTORE_PASSWORD`. Jangan simpan keystore atau kata sandi di repository. Panduan opsi tersedia di [CONFIG.md](CONFIG.md).

## Catatan

- YTRVX adalah proyek pribadi dan bukan aplikasi resmi YouTube, Google, Morphe, atau j-hc.
- Kompatibilitas dapat berubah ketika aplikasi sumber atau patch diperbarui.
- Android tidak mengizinkan APK dengan signature berbeda dipasang menimpa aplikasi yang sudah ada. Bila itu terjadi, uninstall APK YTRVX lama dahulu lalu instal ulang; data aplikasi dapat ikut terhapus.
- Issues hanya untuk masalah builder YTRVX atau file hasil build, bukan permintaan atau bug patch Morphe.

## Kredit

- Builder dasar: [j-hc/revanced-magisk-module](https://github.com/j-hc/revanced-magisk-module)
- Patch: [Morphe Patches](https://github.com/MorpheApp/morphe-patches)
- Patcher: [Morphe Desktop](https://github.com/MorpheApp/morphe-desktop)
