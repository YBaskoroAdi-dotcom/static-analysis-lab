# Laporan Analisis Statis: Easy Level (CrackMe-03)

## Triage
- **Nama Tantangan**: RR7's Secret key
- **Nama File**: `code.exe`
- **Tipe File**: PE32+ executable (console) x86-64 / C++ Application
- **Difficulty**: 1.0 (Easy)
- **Sumber**: https://crackmes.one/crackme/6a46f20c8a86e4c2c5525631

## Strings
Berdasarkan ekstraksi teks statis, ditemukan string karakteristik utama berikut:
- `"Congratulations, you have completed the challenge!"` (Pesan sukses)
- Fungsi input-output dasar seperti format penulisan teks eksekusi.

## Import Table
Biner mengimpor fungsi standar runtime C dari library Windows:
- `printf` (Untuk mencetak output teks ke konsol)
- `scanf` (Untuk menerima input nilai dari pengguna)

## Kesimpulan awal
Biner ini berfungsi sebagai program validasi kode lisensi sederhana berbasis perbandingan nilai *hardcoded*. Alur kendali memeriksa dua tahapan input secara berurutan: input pertama harus bernilai desimal `33` (hex `0x21`) dan input kedua harus bernilai desimal `102` (hex `0x66`). Jika kedua kondisi terpenuhi, program mengarahkan alur eksekusi ke string keberhasilan.