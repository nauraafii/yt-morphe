# Panduan Konfigurasi YTRVX

File [`config.toml`](config.toml) menentukan aplikasi, versi, dan jenis file yang akan dibangun. Ubah sedikit demi sedikit, lalu jalankan workflow build untuk memeriksa hasilnya.

## Mulai cepat

1. Simpan salinan `config.toml` sebelum mengubahnya.
2. Ubah opsi yang diperlukan.
3. Jalankan **Build Modules** dari GitHub Actions.
4. Ambil hasil build dari Releases.

Contoh aplikasi baru:

```toml
[Aplikasi-Baru]
enabled = false
app-name = "Nama Aplikasi"
build-mode = "apk"
version = "auto"
arch = "all"
uptodown-dlurl = "https://contoh.en.uptodown.com/android"
```

Set `enabled = true` hanya ketika aplikasi dan sumber APK sudah benar.

## Opsi umum

| Opsi | Fungsi |
| --- | --- |
| `enable-magisk-update` | Membuat modul Magisk memeriksa pembaruan dari repository ini. |
| `parallel-jobs` | Jumlah proses patch yang berjalan bersamaan. Gunakan `1` untuk build yang lebih ringan. |
| `module-author` | Nama pembuat yang muncul pada metadata modul. |
| `rv-brand` | Nama brand pada file hasil build. Gunakan `YTRVX`. |
| `patches-source` / `patches-version` | Sumber dan versi Morphe Patches. |
| `cli-source` / `cli-version` | Sumber dan versi Morphe Desktop. |
| `dpi` | Urutan DPI yang dicari saat memilih APK. |
| `compression-level` | Kompresi ZIP modul, dari `0` sampai `9`. |
| `remove-rv-integrations-checks` | Biarkan `false` untuk bundle Morphe `.mpp`. |

Versi patcher dapat berupa nomor versi, `latest`, atau `dev`. Konfigurasi YTRVX menggunakan nomor versi agar hasil build lebih mudah diulang.

## Opsi per aplikasi

| Opsi | Fungsi |
| --- | --- |
| `enabled` | Mengikutkan aplikasi dalam build. |
| `app-name` | Nama aplikasi pada hasil build. |
| `build-mode` | `apk`, `module`, atau `both`. |
| `version` | Versi aplikasi: nomor versi, `auto`, `latest`, atau `beta`. |
| `arch` | `all`, `arm64-v8a`, `arm-v7a`, atau `both`. |
| `uptodown-dlurl`, `apkmirror-dlurl`, `archive-dlurl` | Sumber unduhan APK. Minimal satu sumber diperlukan. |
| `included-patches` / `excluded-patches` | Patch yang ingin ditambah atau dilewati. Jika dipakai, tetapkan `version` secara eksplisit. |
| `exclusive-patches` | Hanya memakai patch yang dipilih. Tetapkan `version` secara eksplisit. |
| `include-stock` | Menyertakan APK stok ke ZIP modul. Lebih tahan saat instalasi, tetapi ukuran ZIP bertambah. |
| `module-author` | Mengganti nama pembuat pada metadata modul tertentu. |
| `riplib` | Menghapus library ABI yang tidak dipakai untuk mengurangi ukuran file. |
| `patcher-args` | Opsi tambahan untuk Morphe Patcher. |
| `module-prop-name` | ID modul Magisk. Jangan ubah pada modul yang sudah dipasang jika ingin mempertahankan jalur update. |

## Contoh perubahan sederhana

Untuk membuat hanya APK non-root YouTube, ubah tabel YouTube menjadi:

```toml
[YouTube-Extended]
enabled = true
build-mode = "apk"
```

Untuk menonaktifkan aplikasi dari build, gunakan:

```toml
enabled = false
```

## Catatan patch

- Nama patch yang mengandung tanda petik satu harus ditulis dua kali, misalnya `Hide ''Get Music Premium''`.
- `auto` memilih versi tertinggi yang didukung oleh patch default. Jika memilih atau mengecualikan patch sendiri, gunakan versi aplikasi yang eksplisit agar kompatibilitas tidak ditebak.
- `latest` dan `beta` tidak memeriksa kecocokan patch terlebih dahulu; gunakan hanya jika Anda siap menangani build yang gagal.
- Detail CLI dan bundle `.mpp` tersedia di [Morphe Desktop](https://github.com/MorpheApp/morphe-desktop) dan [Morphe Patches](https://github.com/MorpheApp/morphe-patches).
