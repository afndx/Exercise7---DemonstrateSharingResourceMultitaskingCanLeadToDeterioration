# Program Kontrol LED dengan FreeRTOS di STM32
## Deskripsi
Program ini bertujuan untuk mengontrol empat LED (Hijau, Merah, Kuning, dan Biru) menggunakan mikrokontroler STM32 dan FreeRTOS. Program ini dirancang untuk mengimplementasikan multi-threading, dimana masing-masing LED dikendalikan oleh task yang berbeda dengan waktu delay yang berbeda pula. Sistem ini juga menggunakan mekanisme akses data bersama dengan critical section untuk mencegah konflik antara task yang berjalan.

## Fitur:
- Green LED: Menyala selama 200 ms, dan mematikan LED hijau setelahnya. Menggunakan critical section untuk akses data bersama.
- Red LED: Menyala selama 550 ms, dan mematikan LED merah setelahnya. Menggunakan critical section untuk akses data bersama.
- Yellow LED: LED kuning akan menyala setiap 50 ms, memberikan efek berkedip cepat.
- Blue LED: Menyala jika ada konflik dalam mengakses data bersama (ketika dua task mencoba mengakses data secara bersamaan).
## Struktur Program
1. Inisialisasi Perangkat

- Task FreeRTOS
Program ini menggunakan FreeRTOS untuk membuat beberapa task dengan pengaturan prioritas yang berbeda.

  - defaultTask: Task utama yang berjalan dalam sistem, namun hanya menunggu selama 1 ms.
  - GreenLEDTask: Menyalakan LED Hijau, mengakses data bersama dengan critical section, dan menunggu selama 200 ms.
  -  RedLEDTask: Menyalakan LED Merah, mengakses data bersama dengan critical section, dan menunggu selama 550 ms.
  - YellowLEDTask: Mengaktifkan LED Kuning dengan interval 50 ms.
##  Akses Data Bersama
Data bersama dikelola menggunakan variabel global startFlag yang digunakan untuk memastikan bahwa hanya satu task yang dapat mengakses data dalam satu waktu. Bila ada konflik (dua task mencoba mengakses data bersama secara bersamaan), LED Biru akan menyala untuk menunjukkan terjadinya konflik.

## Simulasi Operasi Baca/Tulis
Fungsi SimulateReadWriteOperation mensimulasikan operasi baca/tulis dengan menggunakan loop kosong (nop), yang membuat mikrokontroler sibuk untuk jangka waktu tertentu.

## Alur Program
- Task GreenLEDTask akan menyala LED Hijau, mengakses data bersama dengan aman, dan mematikannya setelah 200 ms.
- Task RedLEDTask akan melakukan hal yang sama untuk LED Merah, namun dengan delay yang lebih panjang (550 ms).
- Task YellowLEDTask bertugas untuk mengaktifkan LED Kuning dengan interval 50 ms, menciptakan efek kedip.
- Jika terjadi konflik, LED Biru akan menyala menandakan bahwa ada masalah dalam mengakses data bersama.
## Demo
- exercise 7 tanpa disable interrupt
  

https://github.com/user-attachments/assets/669809ab-b6ee-4e24-a417-2decf88c3474

- exercise 7 dengan disable interrupt


https://github.com/user-attachments/assets/023244c8-6c99-4a36-82d3-d5bec0609576


