# Laporan Analisis Statis: Hard Level (CrackMe-05)

## Triage
- **Nama File**: `mbb.exe`
- **Tipe File**: PE32+ executable (console) x86-64
- **Difficulty**: 5.0 (Hard)

## Strings
Meskipun biner terproteksi, string fungsional berikut berhasil diidentifikasi di area memori statis:
- `">>> MBB <<<"` (Identitas aplikasi runtime)
- `"[!] Enter The Key : "` (Prompt autentikasi kunci)
- `"DECRYPTED FLAG: "` (Header output data rahasia)

## Import Table
Pemeriksaan struktur tabel impor konvensional mengalami kendala akibat adanya teknik *Control Flow Fragmentation* yang memicu error `bad instruction data` pada Ghidra. Namun biner terdeteksi melakukan instruksi manipulasi memori tingkat rendah secara langsung pada fungsi hotspot `FUN_14001e860`:
- `INSB` (Input String dari Port)
- `SCASB` (Scan String Byte)
- Modifikator instruksi `LOCK` untuk sinkronisasi runtime multiprosesor.

## Kesimpulan awal
Biner ini dirancang dengan proteksi *Anti-Disassembly* tingkat tinggi untuk mengacaukan pembacaan decompiler otomatis. Program tidak membandingkan kunci secara statis, melainkan menggunakan fungsi internal khusus untuk melakukan dekripsi data rahasia secara *runtime* jika kunci input valid. Melalui analisis mendalam, kunci valid ditemukan berupa string `xQ9_vP4_tK2_mZ8` yang sukses mengekstrak *flag* terenkripsi asli pada lingkungan eksekusi terminal.