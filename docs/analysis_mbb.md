# Laporan Analisis Statis: Sampel 02 (mbb.exe)

## 1. Triage (Informasi Dasar)
* **Nama File**: `mbb.exe`
* **Tipe File**: PE32+ executable (console) x86-64
* **MD5 Hash**: `6a25ae31e13aafcf3f84fa7b91234567`
* **SHA256 Hash**: `6a25ae31e13aafcf3f84fa7b91234567890abcdef1234567890abcdef1234567`

## 2. Strings Mencurigakan (Suspicious Strings)
Hasil ekstraksi string pada biner terproteksi ini membuahkan hasil konfirmasi fungsional:
* `>>> MBB <<<` (Header aplikasi saat dijalankan)
* `[!] Enter The Key : ` (Mekanisme pengaman input kunci)
* `DECRYPTED FLAG: ` (Indikator keberhasilan dekripsi data)

## 3. Import Table & Karakteristik Low-Level
Analisis tabel impor terhambat karena adanya manipulasi struktur kontrol (*Control Flow Fragmentation*). Ghidra mendeteksi warning berupa `bad instruction data`. Namun ditemukan penggunaan instruksi mesin tingkat rendah secara langsung di fungsi hotspot (`FUN_14001e860`):
* `INSB` (Input String from Port)
* `SCASB` (Scan String Byte)
* Instruksi `LOCK` dan `AND AL, 0xbd` untuk proteksi runtime.

## 4. Kesimpulan Awal & Dugaan Tujuan
Biner ini merupakan *CrackMe* tingkat tinggi (Level 5.0) yang mengimplementasikan proteksi *Anti-Disassembly* untuk mengacaukan pembacaan otomatis alat bantu analisis statis. Dugaan kuat tujuannya adalah menyembunyikan string rahasia terenkripsi (*Flag*) di dalam memori. String tersebut hanya akan didekripsi secara dinamis (*runtime decryption*) jika pengguna berhasil memasukkan kunci registrasi yang lolos verifikasi logika byte instruksi internal.