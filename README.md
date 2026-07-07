# Static Analysis Laboratory: PE & ELF Binary Analysis

Repositori ini merupakan dokumentasi komprehensif hasil praktikum mata kuliah *Reverse Engineering* (Semester 4). Fokus utama repositori ini adalah melakukan pembedahan statis (*static analysis*) terhadap berbagai sampel biner, mulai dari tantangan *CrackMe* hingga analisis *real-world malware*.

## Struktur Repositori
* `docs/` : Berisi laporan analisis mendalam untuk setiap sampel biner.
    * `analisis_easy.md` : Analisis tantangan tingkat *Easy* (`code.exe`).
    * `analisis_medium.md` : Analisis tantangan tingkat *Medium* (`Untitled3.exe`).
    * `analisis_hard.md` : Analisis tantangan tingkat *Hard* (`mbb.exe`).
    * `analisis_cryptojacking_elf.md` : Analisis biner *Malware Cryptojacking* (y11.elf).

## Metodologi Analisis
Setiap laporan disusun mengikuti standar struktur pelaporan laboratorium yang wajib:
1. **Triage**: Identifikasi metadata, hash (SHA256/MD5), dan arsitektur biner.
2. **Strings**: Ekstraksi teks karakter statis untuk menemukan Indikator Kompromi (IoC).
3. **Import Table**: Analisis dependensi pustaka sistem dan fungsi API yang digunakan.
4. **Kesimpulan awal**: Sintesis tujuan fungsional dan perilaku biner berdasarkan temuan teknis.

## Informasi Penulis
* **Nama**: Yudhistira Baskoro Adi Admojo (Sri Yulianingsih Agustin)
* **Mata Kuliah**: Reverse Engineering
* **Status Lab**: Lengkap (Analisis Biner PE & ELF)

## Aturan Keamanan
Seluruh file biner asli **tidak diunggah** ke dalam repositori ini untuk mencegah eksekusi yang tidak disengaja. Repositori ini murni untuk tujuan riset edukasi keamanan siber.

---
*Dibuat untuk kebutuhan tugas praktikum Reverse Engineering - Universitas Amikom Yogyakarta 2026.*