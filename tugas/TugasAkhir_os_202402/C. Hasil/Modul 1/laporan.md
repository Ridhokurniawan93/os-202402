
# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi  
**Semester**: Genap / Tahun Ajaran 2024–2025  
**Nama**: `Ridho Kurniawan 
**NIM**: `240202881  
**Modul yang Dikerjakan**: Modul 1 – System Call dan Instrumentasi Kernel

---

## 📌 Deskripsi Singkat Tugas

Modul 1 ini berfokus pada penambahan dua system call baru pada kernel xv6-public (x86), yaitu `getpinfo()` untuk memperoleh informasi proses yang sedang aktif, serta `getReadCount()` untuk memonitor jumlah pemanggilan fungsi `read()` sejak sistem booting. Modul ini menguji pemahaman mahasiswa terhadap arsitektur syscall, struktur kernel, serta mekanisme modifikasi kernel-level dan user-level secara bersamaan.

---

## 🛠️ Rincian Implementasi

Langkah-langkah yang dilakukan untuk menyelesaikan modul ini meliputi:

- Menambahkan struktur `struct pinfo` di `proc.h` untuk menyimpan informasi proses.
- Menambahkan counter `readcount` global di `sysproc.c` untuk mencatat jumlah pemanggilan `read()`.
- Memperluas file `syscall.h`, `user.h`, `usys.S`, dan `syscall.c` agar mendukung syscall baru.
- Mengimplementasikan fungsi kernel `sys_getpinfo()` dan `sys_getreadcount()` di `sysproc.c`.
- Memodifikasi `sysfile.c` agar `readcount` bertambah tiap kali `read()` dipanggil.
- Membuat dua program uji: `ptest.c` untuk `getpinfo()` dan `rtest.c` untuk `getreadcount()`.
- Menambahkan program uji ke dalam `Makefile`.

---

## ✅ Uji Fungsionalitas

Program uji yang digunakan untuk menguji fungsionalitas sebagai berikut:

- `ptest`: Mengekstrak dan menampilkan informasi proses (PID, ukuran memori, dan nama proses) dari `getpinfo()`.
- `rtest`: Menampilkan nilai `readcount` sebelum dan sesudah operasi `read()` dilakukan pada stdin.

---

## 📷 Hasil Uji

### 📍 Output `ptest`

```
PID	MEM	NAME
1	4096	init
2	2048	sh
3	2048	ptest
```

### 📍 Output `rtest`

```
Read Count Sebelum: 4
hello
Read Count Setelah: 5
```
### Screenshot
![hasil rtest dan ptest](./screnshot/modul1.png)

---

## ⚠️ Kendala yang Dihadapi

Beberapa tantangan yang ditemukan selama implementasi:

- Struktur `ptable.proc` tidak tersedia secara langsung di `xv6-public`, sehingga perlu memastikan pointer `struct proc *p` mengakses proses dari array `proc[]` global yang ada.
- Potensi race condition saat membaca daftar proses, diselesaikan dengan penguncian menggunakan `acquire(&ptable.lock)` dan `release(&ptable.lock)`.
- Perlu berhati-hati dalam menyusun indeks array `pinfo` agar tidak melewati batas dan menyebabkan error kernel.

---

## 📚 Referensi

- MIT xv6 Book. *xv6: a simple, Unix-like teaching operating system*, [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
- Repositori GitHub xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
- Diskusi dan dokumentasi praktikum Sistem Operasi (2025)
- Stack Overflow dan forum diskusi kernel open source
