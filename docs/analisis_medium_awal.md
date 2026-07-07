# Laporan Analisis Statis: Medium Level (CrackMe-04)

## Triage
- **Nama File**: `Untitled3.exe`
- **Tipe File**: PE32+ executable (console) x86-64 / C++ Application
- **Difficulty**: 2.2 (Medium)

## Strings
Hasil ekstraksi string menggunakan Ghidra menunjukkan beberapa pengenal interaksi aktif:
- `"user: "` (Instruksi input nama pengguna)
- `"password: "` (Instruksi input kata sandi)
- `"GOOD!"` (Indikator kecocokan lisensi)
- `"BAD >:("` (Indikator kegalangan lisensi)

## Import Table
Program memanfaatkan library standar dinamis `KERNEL32.dll` beserta fungsi standard library C++ (`std::string`):
- `std::string::length` (Untuk menghitung jumlah karakter input)
- `std::string::operator[]` (Untuk melakukan iterasi array karakter)

## Kesimpulan awal
Program ini menerapkan mekanisme pemeriksaan *checksum* dinamis berbasis nama pengguna (*username*). Setiap karakter dari *username* diiterasi di dalam loop, dikalikan dengan konstanta `4`, lalu diakumulasikan ke memori. Hasil akhir perhitungan ini wajib sama dengan *password* yang dimasukkan pengguna agar memicu output `"GOOD!"`. Skema ini berhasil dibuktikan secara dinamis melalui kombinasi input `abcd` dan `1576`.