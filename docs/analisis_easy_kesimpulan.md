# Laporan Analisis Statis: Easy Level (CrackMe-03)

## Triage
- **Nama Tantangan**: RR7's Secret key
- **Nama File**: `code.exe`
- **Tipe File**: PE32+ executable (console) x86-64 / C++ Application
- **Arsitektur**: x86-64 (64-bit biner)
- **Difficulty**: 1.0 (Easy)
- **Sumber Sampel**: https://crackmes.one/crackme/6a46f20c8a86e4c2c5525631

## Strings
Melalui pemindaian statis pada block memori read-only data (`.rodata`), ditemukan string literal yang menjadi Indikator Kompromi (IoC) fungsional biner:
- `"Congratulations, you have completed the challenge!"` (String penanda keberhasilan eksploitasi).
- Karakter petunjuk interaksi input-output yang digunakan program untuk memandu pengguna di terminal.

## Import Table
Biner ini mengandalkan pustaka standard C runtime (`libc`) untuk menjembatani komunikasi dasar antara program dan sistem operasi:
- `printf`: Berfungsi untuk mencetak instruksi teks pembuka ke layar konsol pengguna.
- `scanf`: Berfungsi untuk menangkap arus input data numerik desimal yang dimasukkan oleh pengguna secara langsung melalui *keyboard*.

## Kesimpulan
Berdasarkan hasil dekompilasi statis menggunakan Ghidra, biner ini diidentifikasi sebagai program tantangan lisensi (*CrackMe*) mendasar yang menerapkan mekanisme pengaman berbasis perbandingan nilai statis (*hardcoded comparison*) di dalam memori. Program menggunakan struktur kendali percabangan kondisional (`CMP` dan instruksi lompatan *conditional jump*) yang mengevaluasi input pengguna melalui dua tahapan verifikasi yang sekuensial. Input pertama diwajibkan bernilai desimal **33** (hex `0x21`), dan input kedua harus bernilai desimal **102** (hex `0x66`). Apabila kedua angka ini dimasukkan dengan benar, alur eksekusi akan diarahkan menuju pesan keberhasilan. Proteksi ini sangat minimal karena tidak menggunakan enkripsi, sehingga sangat rentan terhadap analisis memori statis.