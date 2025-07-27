
# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi  
**Semester**: Genap / Tahun Ajaran 2024–2025  
**Nama**: Ridho Kurniawan  
**NIM**: 240202881

**Modul yang Dikerjakan**: Modul 2 – Penjadwalan CPU Lanjutan (Priority Scheduling Non-Preemptive)

---

## 📌 Deskripsi Singkat Tugas

Modul ini bertujuan untuk mengganti algoritma penjadwalan CPU default xv6 dari Round Robin menjadi Non-Preemptive Priority Scheduling. Dalam pendekatan ini, proses dengan prioritas numerik terkecil akan dijalankan lebih dahulu, dan proses lainnya harus menunggu hingga proses yang berjalan selesai. Prioritas diatur menggunakan syscall baru `set_priority(int)`.

---

## 🛠️ Rincian Implementasi

- Menambahkan field `priority` di `struct proc` (`proc.h`)
- Inisialisasi default priority di `allocproc()` (`proc.c`)
- Menambahkan syscall baru `set_priority()`:
  - Nomor syscall (`syscall.h`)
  - Deklarasi (`user.h`)
  - Entri syscall (`usys.S`)
  - Registrasi di `syscall.c`
  - Implementasi di `sysproc.c`
- Memodifikasi `scheduler()` di `proc.c` agar memilih proses dengan prioritas tertinggi (nilai terkecil)
- Membuat program penguji `ptest.c`
- Menambahkan `ptest` ke dalam `Makefile`

---

## ✅ Uji Fungsionalitas

Program `ptest` membuat dua proses anak dengan prioritas berbeda, lalu mengevaluasi urutan eksekusi berdasarkan prioritas.

```bash
$ ptest
Child 2 selesai   // prioritas tinggi → selesai duluan
Child 1 selesai   // prioritas rendah
Parent selesai
```

---

## 📷 Cuplikan Kode Pengujian

```c
int pid1 = fork();
if (pid1 == 0) {
  set_priority(90);
  busy();
  printf(1, "Child 1 selesai\n");
  exit();
}

int pid2 = fork();
if (pid2 == 0) {
  set_priority(10);
  busy();
  printf(1, "Child 2 selesai\n");
  exit();
}
```
![hasil cowtest](./screnshot/modul2.png)
---

## ⚠️ Kendala yang Dihadapi

- Scheduler asli di xv6 menggunakan pendekatan Round Robin, sehingga perlu penggantian menyeluruh terhadap logika pemilihan proses.
- Pastikan semua proses yang tidak dalam state RUNNABLE diabaikan.
- Validasi input syscall `set_priority` untuk menghindari nilai tidak valid.

---

## 📚 Referensi

- MIT xv6 Book. *xv6: a simple, Unix-like teaching operating system*  
  [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
- Repositori GitHub xv6-public:  
  [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)

---

## 📌 Kesimpulan

Dengan implementasi ini:

- Berhasil menambahkan field kernel, syscall baru, dan logika penjadwalan berbasis prioritas
- Scheduler hanya menjalankan proses dengan prioritas tertinggi dan tidak melakukan preemption
- Eksperimen ini memperlihatkan pengaruh kebijakan penjadwalan terhadap perilaku sistem secara langsung
