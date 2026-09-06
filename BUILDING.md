# Panduan Build YTRVX

Dokumen ini untuk pemilik fork yang ingin mengubah konfigurasi atau membuat rilis sendiri. Untuk memilih aset rilis, lihat [README.md](README.md); untuk arti setiap opsi, lihat [CONFIG.md](CONFIG.md).

## Pilih jalur build

| Jalur | Kapan dipakai | Dampak |
| --- | --- | --- |
| GitHub Actions | Rilis fork sendiri | Menggunakan secret repository, membuat atau memperbarui GitHub Release. |
| Linux atau Termux lokal | Menguji perubahan secara lokal | Tetap membutuhkan keystore pribadi dan lingkungan Bash. Build lokal tidak mengaktifkan update URL Magisk. |
| `build-termux.sh` | Menyiapkan Termux dengan alur terpandu | Memperbarui paket, dan dapat mengganti clone lokal yang tertinggal setelah menyimpan `config.toml`. Tinjau script sebelum menjalankannya. |

## Build rilis dengan GitHub Actions

Workflow [`Build Modules`](.github/workflows/build.yml) menyiapkan Java 21, memanggil `build.sh`, membuat `SHA256SUMS.txt`, dan mengunggah seluruh aset ke GitHub Release.

Sebelum menjalankannya, repositori fork memerlukan dua secret:

| Secret | Isi | Digunakan untuk |
| --- | --- | --- |
| `YTRVX_KEYSTORE_B64` | Keystore PKCS#12 yang sudah di-Base64 | Signing APK pada runner GitHub. |
| `YTRVX_KEYSTORE_PASSWORD` | Password keystore dan entry `ytrvx` | Membuka keystore saat signing. |

Jangan menaruh nilai secret di `config.toml`, issue, log, commit, atau file yang diunggah. Setelah secret tersedia, jalankan workflow dari branch `main` dan periksa log serta aset rilis yang dibuat.

## Build lokal

Builder adalah script Bash. Gunakan Linux, Termux, atau shell Bash yang kompatibel; PowerShell murni tidak menjalankan `build.sh` secara langsung.

Prasyarat minimum yang diperiksa atau digunakan builder:

- Java 21 atau lebih baru;
- Bash, `curl`, `jq`, dan `zip`;
- Git clone repository beserta submodule;
- keystore PKCS#12 pribadi dengan alias `ytrvx`;
- variabel lingkungan `YTRVX_KEYSTORE_PATH` dan `YTRVX_KEYSTORE_PASSWORD`.

Morphe Desktop juga mendokumentasikan Java 21+ sebagai prasyarat CLI. Lihat [dokumentasi resminya](https://github.com/MorpheApp/morphe-desktop#prerequisites) sebelum mengganti versi patcher atau bundle.

Contoh alur di Linux atau Termux:

```bash
git clone --recurse-submodules https://github.com/nauraafii/ytrvx-module.git
cd ytrvx-module

export YTRVX_KEYSTORE_PATH="$PWD/ytrvx-release.p12"
read -rs -p "Keystore password: " YTRVX_KEYSTORE_PASSWORD
export YTRVX_KEYSTORE_PASSWORD

./build.sh config.toml
```

Hasil lokal ditulis ke direktori `build/`, yang sengaja diabaikan oleh Git. Simpan keystore di lokasi aman di luar repositori jika memungkinkan. Jangan memakai keystore rilis orang lain: pembaruan Android harus mempertahankan identitas penandatanganan yang sama.

## Menyiapkan keystore sendiri

Builder memanggil Morphe dengan alias keystore `ytrvx`. Buat keystore PKCS#12 dengan alias tersebut, simpan password secara aman, lalu ubah `YTRVX_KEYSTORE_PATH` agar menunjuk ke file itu. Referensi sintaks dan opsi `keytool` tersedia di [dokumentasi Java](https://docs.oracle.com/en/java/javase/21/docs/specs/man/keytool.html).

Keystore adalah identitas rilis. Bila hilang atau diganti, Android menganggap APK baru memiliki signature berbeda; pengguna biasanya harus menghapus pemasangan versi sebelumnya sebelum memasang versi baru.

## Helper Termux

[`build-termux.sh`](build-termux.sh) memasang dependensi `git`, `curl`, `jq`, `openjdk-21`, dan `zip`, lalu menjalankan `build.sh`. Pada setup pertama, aplikasi meminta akses penyimpanan dan menempatkan `config.toml` serta output di `/sdcard/Download/ytrvx-module`.

Script ini dirancang untuk penggunaan pribadi dan dapat memperbarui paket Termux. Jangan jalankan tanpa membaca isi script; gunakan [GitHub Actions](#build-rilis-dengan-github-actions) jika tujuan Anda adalah membuat rilis fork yang dapat direproduksi.
