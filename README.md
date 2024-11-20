# Pengendalian LED STM32 Menggunakan FreeRTOS
Program ini menunjukkan bagaimana cara mengendalikan empat LED pada mikrokontroler STM32 menggunakan FreeRTOS. Setiap LED dikendalikan oleh task terpisah yang berjalan secara paralel. Program ini menggunakan fitur FreeRTOS seperti pembuatan task, penjadwalan, dan sinkronisasi antar-task untuk mengendalikan task LED secara bersamaan.

## Fitur Utama
- Empat task kontrol LED terpisah (Green, Red, Yellow, dan Blue LED)
- Perubahan status LED dengan frekuensi dan perilaku yang berbeda
- Penggunaan critical section untuk melindungi sumber daya bersama (startFlag)
- FreeRTOS untuk multi-threading
## Deskripsi Perangkat Lunak
### Inisialisasi Utama (main.c)
- Inisialisasi HAL: Program dimulai dengan inisialisasi hardware abstraction layer (HAL) STM32, yang mengonfigurasi clock sistem, menginisialisasi GPIO, dan memulai FreeRTOS.
### Pembuatan Task FreeRTOS:
- Default Task: Task ini menjalankan loop minimal yang tidak melakukan apa-apa kecuali memanggil osDelay() untuk memberi kesempatan scheduler untuk berjalan.
- Task LED Hijau (greend_led): Task ini menyalakan LED Hijau (GPIO_PIN_0), mengakses data bersama dengan critical section, dan kemudian mematikan LED setelah penundaan 200 ms.
- Task LED Merah (red_led): Mirip dengan task LED Hijau, tetapi mengendalikan LED Merah (GPIO_PIN_1) dan task ini memiliki penundaan 550 ms.
- Task LED Kuning (yellow_led): Task ini menyalakan dan mematikan LED Kuning (GPIO_PIN_2) setiap 50 ms, menghasilkan efek kedipan.
### Manajemen Critical Section
Task untuk LED Hijau dan Merah menunjukkan akses ke data bersama (startFlag). Fungsi accessSharedData() memastikan bahwa hanya satu task yang dapat mengakses sumber daya bersama pada satu waktu dengan menggunakan critical section (taskENTER_CRITICAL dan taskEXIT_CRITICAL). Ini mencegah terjadinya konflik saat mengakses atau memodifikasi data bersama.

- Critical Section:
   - Jika startFlag == 1, flag diubah menjadi 0, yang menunjukkan bahwa sumber daya bersama sedang digunakan.
   - Jika flag bernilai 0, ini menandakan adanya konflik, dan LED Biru (GPIO_PIN_3) akan menyala untuk memberi sinyal masalah.
   - Setelah operasi simulasi (diwakili oleh penundaan 1000 ms), flag direset menjadi 1, dan LED Biru dimatikan.
### Inisialisasi GPIO (MX_GPIO_Init)
Pin GPIO yang digunakan untuk LED diinisialisasi dengan mode output push-pull. Pin-pin berikut dikonfigurasi sebagai output:
- LED1 (Hijau) pada GPIO_PIN_0
- LED2 (Merah) pada GPIO_PIN_1
- LED3 (Kuning) pada GPIO_PIN_2
- LED4 (Biru) pada GPIO_PIN_3
### Konfigurasi Clock Sistem (SystemClock_Config)
Clock sistem dikonfigurasi untuk menggunakan osilator Internal High-Speed (HSI) untuk menggerakkan CPU, AHB, dan bus APB. Ini memastikan mikrokontroler beroperasi pada frekuensi clock yang sesuai dengan task.

### Konfigurasi FreeRTOS
Program ini menggunakan FreeRTOS untuk mengelola task secara bersamaan:

- Task: Setiap LED dikendalikan oleh task FreeRTOS terpisah, memungkinkan eksekusi paralel.
- Scheduler: Scheduler FreeRTOS menangani eksekusi task secara real-time, memungkinkan LED untuk berkedip dan menyala pada frekuensi yang berbeda sesuai dengan task-nya.
- Penundaan: Fungsi osDelay() digunakan di setiap task untuk memperkenalkan penundaan, memungkinkan task lainnya berjalan sementara menunggu.
### Deskripsi Fungsi
- main(): Menginisialisasi sistem, mengonfigurasi clock, GPIO, dan membuat task FreeRTOS.
- StartDefaultTask(): Task default dengan fungsionalitas minimal untuk menjaga sistem tetap berjalan.
- greend_led(): Menyalakan LED Hijau, mengakses data bersama dalam critical section, lalu mematikan LED.
- red_led(): Mirip dengan task LED Hijau, tetapi mengendalikan LED Merah dan memiliki penundaan yang lebih lama.
- yellow_led(): Menyalakan dan mematikan LED Kuning setiap 50 ms.
- accessSharedData(): Mensimulasikan akses ke data bersama, dilindungi oleh critical section, dan menangani konflik dengan menyalakan LED Biru jika terjadi konflik.
### Penanganan Error
- Error_Handler(): Jika terjadi error, fungsi ini menonaktifkan interupsi dan memasuki loop tak terbatas.
- assert_failed(): Digunakan untuk debugging, melaporkan file dan nomor baris di mana terjadi kegagalan assert.Pengendalian LED STM32 Menggunakan FreeRTOS
## Demo
 

https://github.com/user-attachments/assets/fb6f391d-3859-4eac-a9ab-fb13831d0019

