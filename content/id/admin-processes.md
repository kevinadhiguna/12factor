## XII. Admin processes
### Jalankan tugas admin/management sebagai one-off processes

[Formasi Proses](./concurrency) adalah array proses yang digunakan untuk melakukan bisnis reguler aplikasi (seperti menangani permintaan web) saat dijalankan. Secara terpisah, pengembang sering kali ingin melakukan one-off administrative atau maintanance tasks untuk aplikasi, seperti:

* Menjalankan migrasi basis data (misalnya `manage.py migrate` di Django, `rake db:migrate` di Rails).
* Menjalankan console (dikenal juga sebagai shell [REPL](http://en.wikipedia.org/wiki/Read-eval-print_loop)) untuk menjalankan arbitrary code atau memeriksa model aplikasi terhadap basis data langsung. Sebagian besar bahasa menyediakan REPL dengan menjalankan interpreter tanpa argumen apa pun (mis. `python` atau `perl`) atau dalam beberapa kasus memiliki perintah terpisah (mis. `irb` untuk Ruby, `rails console` untuk Rails).
* Menjalankan one-time scripts yang di-commit ke dalam repo aplikasi (mis. `php scripts/fix_bad_records.php`).

One-off admin processes harus dijalankan di lingkungan yang identik dengan [proses yang berjalan lama](./processes) aplikasi. Mereka dijalankan terhadap [rilis](./build-release-run), menggunakan [basis kode](./codebase) dan [config](./config) yang sama seperti proses apa pun yang dijalankan terhadap rilis tersebut. Code admin harus dikirimkan bersama dengan code aplikasi untuk menghindari masalah sinkronisasi.

Teknik [isolasi dependensi](./dependencies) yang sama harus digunakan pada semua jenis proses. Misalnya, jika proses web Ruby menggunakan perintah `bundle exec thin start`, migrasi basis data harus menggunakan `bundle exec rake db:migrate`. Demikian pula, program Python yang menggunakan Virtualenv harus menggunakan `bin/python` yang sudah jadi untuk menjalankan kedua webserver Tornado dan segala proses admin `manage.py`.

Twelve-factor sangat mendukung bahasa yang menyediakan REPL shell secara langsung, dan yang membuatnya mudah untuk menjalankan one-off scripts. Dalam deployment lokal, pengembang menjalankan one-off admin processes dengan perintah shell langusng di dalam direktori checkout aplikasi. Dalam deployment produksi, pengembang dapat menggunakan ssh atau mekanisme eksekusi perintah jarak jauh yang disediakan oleh lingkungan eksekusi deployment untuk menjalankan proses tersebut.
