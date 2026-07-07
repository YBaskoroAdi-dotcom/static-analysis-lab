# Laporan Analisis Statis: Easy Level (CrackMe-03)  

## Triage
- **Nama Tantangan**: RR7's Secret key
- **Nama File**: `code.exe`
- **Tipe File**: PE32+ executable (console) x86-64 / C++ Application
- **Arsitektur**: x86-64 (64-bit biner)
- **Difficulty**: 1.0 (Easy)
- **Sumber Sampel**: https://crackmes.one/crackme/6a46f20c8a86e4c2c5525631

## Strings
Melalui pemindaian statis pada block memori read-only data (`.rodata`), berhasil diekstraksi beberapa string literal yang menjadi Indikator Kompromi (IoC) fungsional biner:
- `"Congratulations, you have completed the challenge!"` (String penanda keberhasilan eksploitasi)
- Karakter petunjuk interaksi input-output yang digunakan program untuk memandu pengguna di terminal.

## Import Table
Biner ini mengandalkan pustaka standard C runtime (`msvcrt.dll` / `libc`) untuk menjembatani komunikasi dasar antara program dan sistem operasi:
- `printf`: Berfungsi untuk mencetak instruksi teks pembuka ke layar konsol pengguna.
- `scanf`: Berfungsi untuk menangkap arus input data numerik desimal yang dimasukkan oleh pengguna secara langsung melalui *keyboard*.

## Kesimpulan awal
Berdasarkan hasil dekompilasi statis menggunakan Ghidra, biner ini diidentifikasi sebagai program tantangan lisensi (*CrackMe*) mendasar yang menerapkan mekanisme pengaman berbasis perbandingan nilai statis (*hardcoded comparison*) di dalam memori. Program ini menggunakan struktur kendali percabangan kondisional (`CMP` dan instruksi lompatan *conditional jump*) yang mengevaluasi input pengguna melalui dua tahapan verifikasi yang ketat dan sekuensial. 

Input pertama secara mutlak diwajibkan bernilai heksadesimal `0x21` yang setara dengan angka desimal **33**. Jika kondisi pertama terpenuhi, program akan melanjutkan ke pemeriksaan tahap kedua, di mana nilai input berikutnya harus cocok dengan heksadesimal `0x66` atau desimal **102**. Apabila kedua tahapan angka magis ini dimasukkan secara berurutan dengan benar, alur eksekusi biner akan secara otomatis dialihkan untuk memicu fungsi pencetakan string keberhasilan tantangan. Sebaliknya, kegagalan pada salah satu tahap akan langsung menghentikan program atau mengarahkannya pada percabangan gagal. Melalui pemetaan statis ini, dapat disimpulkan bahwa proteksi biner ini sangat minim karena tidak melibatkan algoritma enkripsi atau teknik pengacakan kode (*obfuscation*), sehingga sangat rentan terhadap analisis memori statis konvensional.