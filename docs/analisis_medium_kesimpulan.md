# Laporan Analisis Statis: Medium Level (CrackMe-04)

## Triage
- **Nama File**: `Untitled3.exe`
- **Tipe File**: PE32+ executable (console) x86-64 / C++ Application
- **Difficulty**: 2.2 (Medium)

## Strings
Hasil ekstraksi string menggunakan Ghidra menunjukkan pengenal interaksi yang aktif dan spesifik:
- `"user: "` (Instruksi input nama pengguna)
- `"password: "` (Instruksi input kata sandi)
- `"GOOD!"` (Indikator keberhasilan validasi lisensi)
- `"BAD >:("` (Indikator kegagalan validasi lisensi)

## Import Table
Program memanfaatkan library standar dinamis `KERNEL32.dll` beserta fungsi standard library C++ (`std::string`):
- `std::string::length`: Digunakan untuk menentukan batasan loop saat menghitung jumlah karakter input pengguna.
- `std::string::operator[]`: Digunakan sebagai sarana untuk melakukan iterasi per karakter pada buffer memori *username*.

## Kesimpulan
Biner ini menerapkan mekanisme pemeriksaan *checksum* dinamis berbasis nama pengguna (*username-dependent validation*). Alih-alih membandingkan string secara langsung, program melakukan iterasi di dalam sebuah *loop* terhadap setiap karakter dari *username* yang dimasukkan. Dalam setiap iterasi, nilai ASCII karakter tersebut dikalikan dengan konstanta `4` (melalui operasi *bitwise shift left*) dan diakumulasikan ke dalam variabel memori. Hasil akhir perhitungan ini harus sesuai dengan angka yang dimasukkan di kolom *password* agar program mencetak pesan `"GOOD!"`. Skema ini berhasil dibuktikan melalui dekonstruksi logika loop C++ yang menunjukkan kebergantungan input *password* terhadap input *username*, dan telah diverifikasi secara dinamis dengan kombinasi input `abcd` dan `1576`.