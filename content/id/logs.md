## XI. Logs
### Perlakukan log sebagai aliran event

*Log* memberikan visibilitas ke perilaku aplikasi yang sedang berjalan. Dalam lingkungan berbasis server, mereka biasanya ditulis ke file pada disk ("logfile"); Tetapi ini hanya format keluaran.

Log adalah [aliran](https://adam.herokuapp.com/past/2011/4/1/logs_are_streams_not_files/) dari peristiwa teragregasi dengan urutan waktu yang dikumpulkan dari aliran keluaran dari semua proses yang berjalan dan backing services. Log dalam bentuk mentahnya biasanya dalam format teks dengan satu peristiwa per baris (meskipun backtrace dari pengecualian dapat mencakup beberapa baris). Log tidak memiliki awal atau akhir yang tetap tetapi mengalir terus selama aplikasi terus beroperasi.

**Twelve-factor app tidak pernah mementingkan routing atau penyimpanan aliran keluarannya.** Aplikasi tidak boleh mencoba menulis atau mengelola file log. Sebagai gantinya, setiap proses yang berjalan menulis aliran peristiwanya, tanpa buffer, ke `stdout`. Selama pengembangan lokal, pengembang akan melihat aliran ini di latar depan terminal mereka untuk mengamati perilaku aplikasi.

Dalam tahap staging atau deployment tahap production, setiap aliran proses akan ditangkap oleh lingkungan eksekusi, disusun bersama dengan semua aliran lain dari aplikasi, dan dialihkan ke satu atau beberapa tujuan akhir untuk dilihat dan pengarsipan jangka panjang. Tujuan arsip ini tidak terlihat atau dapat dikonfigurasi oleh aplikasi, dan sebaliknya dikelola oleh lingkungan eksekusi. Open-source log router (seperti [Logplex](https://github.com/heroku/logplex) dan [Fluentd](https://github.com/fluent/fluentd)) tersedia untuk tujuan ini.

Aliran peristiwa untuk aplikasi dapat dirutekan ke file, atau ditonton melalui tail dalam terminal secara realtime. Yang paling signifikan, aliran dapat dikirim ke sistem pengindeksan dan analisis log seperti [Splunk](http://www.splunk.com/), atau sistem data warehousing tujuan umum seperti [Hadoop/Hive](http://hive.apache.org/). Sistem ini memungkinkan kekuatan dan fleksibilitas yang besar untuk melakukan introspeksi terhadap perilaku aplikasi dari waktu ke waktu, termasuk:

* Menemukan peristiwa tertentu di masa lalu.
* Grafik tren skala besar (seperti permintaan per menit).
* Peringatan aktif menurut heuristik yang ditentukan pengguna (seperti peringatan ketika jumlah error per menit melebihi ambang tertentu).
