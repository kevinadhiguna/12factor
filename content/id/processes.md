## VI. Processes
### Jalankan aplikasi sebagai satu atau beberapa proses stateless

Aplikasi dijalankan di lingkungan eksekusi sebagai satu atau beberapa *proses*.

Dalam kasus paling sederhana, kodenya adalah stand-alone script, lingkungan eksekusi adalah laptop lokal pengembang dengan runtime bahasa yang ter-install, dan prosesnya diluncurkan mealui command line (contohnya, `python my_script.py`). Di ujung lain spektrum, deploy tahap produksi dari aplikasi canggih dapat menggunakan [jenis proses, yang dibuat menjadi nol atau beberapa proses yang berjalan](./concurrency).

**Proses Twelve-factor bersifat stateless dan [share-nothing](http://en.wikipedia.org/wiki/Shared_nothing_architecture).**  Setiap data yang perlu dipertahankan harus disimpan dalam sebuah stateful [backing service](./backing-services), biasanya basis data.

Ruang memori atau filesystem dari proses dapat digunakan sebagai transaksi tunggal cache yang singkat. Contohnya, mengunduh file besar, mengoperasikannya, dan menyimpan hasil operasi di basis data. Twelve-factor app tidak pernah mengasumsikan bahwa apa pun yang disimpan dalam cached dalam memori atau pada disk akan tersedia pada permintaan atau pekerjaan di masa mendatang -- dengan banyak proses yang berjalan, kemungkinan besar bahwa permintaan di masa mendatang akan dilayani oleh proses yang berbeda. Bahkan ketika hanya menjalankan satu proses, restart (dipicu oleh kode yang di-deploy, perubahan konfigurasi, atau lingkungan eksekusi yang memindahkan proses ke lokasi fisik yang berbeda) akan menghapus semua status lokal (misalnya, memori dan filesystem).

Asset packagers seperti [django-assetpackager](http://code.google.com/p/django-assetpackager/) menggunakan filesystem sebagai cache untuk aset yang dikompilasi. Twelve-factor app lebih memilih melakukan kompilasi ini selama [tahap build](/build-release-run). Asset packagers seperti [Jammit](http://documentcloud.github.com/jammit/) dan [Rails asset pipeline](http://ryanbigg.com/guides/asset_pipeline.html) dapat dikonfigurasi untuk mengemas aset selama tahap build.

Beberapa sistem web mengandalkan ["sticky sessions"](http://en.wikipedia.org/wiki/Load_balancing_%28computing%29#Persistence) -- yaitu, menyimpan data session pengguna ke dalam memori proses aplikasi dan mengharapkan permintaan dari pengunjung yang sama untuk diarahkan ke proses yang sama. Sticky sessions merupakan pelanggaran twelve-factor seharusnya tidak boleh digunakan atau diandalkan. State Session data adalah kandidat baik untuk menyimpan data yang menawarkan waktu kadaluarsa, seperti [Memcached](http://memcached.org/) atau [Redis](http://redis.io/).
