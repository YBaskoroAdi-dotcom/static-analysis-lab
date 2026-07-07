# Laporan Analisis Statis: Hard Level (CrackMe-05)

## Triage
- **Nama File**: `mbb.exe`
- **Tipe File**: PE32+ executable (console) x86-64
- **Difficulty**: 5.0 (Hard)

## Strings
Meskipun biner terproteksi, string fungsional berikut berhasil diidentifikasi di area memori statis:
- `">>> MBB <<<"` (Identitas aplikasi saat runtime)
- `"[!] Enter The Key : "` (Prompt autentikasi kunci pengguna)
- `"DECRYPTED FLAG: "` (Header output untuk data rahasia setelah proses dekripsi)

## Import Table
Pemeriksaan struktur tabel impor konvensional mengalami kendala akibat teknik *Control Flow Fragmentation* yang memicu galat `bad instruction data` pada Ghidra. Namun, biner terdeteksi melakukan instruksi manipulasi memori tingkat rendah secara langsung di fungsi hotspot `FUN_14001e860`:
- `INSB` (Input Byte from Port to String): Membaca data langsung dari port sistem.
- `SCASB` (Scan String Byte): Digunakan untuk melakukan pemindaian buffer memori secara efisien.
- `LOCK`: Modifikator instruksi untuk sinkronisasi akses memori tingkat rendah.

## Kesimpulan
Biner ini dirancang dengan proteksi *Anti-Disassembly* tingkat tinggi untuk mengacaukan pembacaan decompiler otomatis. Program tidak membandingkan kunci secara statis, melainkan menggunakan fungsi internal khusus untuk melakukan dekripsi data rahasia (*flag*) secara *runtime*. Melalui analisis mendalam terhadap alur *cross-reference* (XREF) dan eksekusi instruksi tingkat rendah, kunci valid berhasil ditemukan yaitu `xQ9_vP4_tK2_mZ8`. Kunci tersebut sukses memicu fungsi dekripsi internal dan menghasilkan *decrypted flag* asli pada lingkungan eksekusi terminal.