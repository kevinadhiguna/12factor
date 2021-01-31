## VIII. Concurrency
### Skalakan melalui model process

Setiap program komputer, setelah dijalankan, diwakili oleh satu atau lebih proses. Aplikasi web telah mengambil berbagai bentuk eksekusi proses. Contohnya, proses PHP dijalankan sebagai proses turunan dari Apache, dimulai sesuai permintaan sesuai yang dibutuhkan volume permintaan. Proses Java mengambil pendekatan yang berlawanan, dengan JVM menyediakan satu uberprocess besar yang mencadangkan blok besar sumber daya sistem (CPU dan memori) saat startup, dengan konkurensi yang dikelola secara internal melalui threads. Dalam kedua kasus tersebut, proses yang berjalan hanya terlihat secara minimal oleh pengembang aplikasi.

![Skala dinyatakan sebagai proses yang berjalan, keragaman beban kerja dinyatakan sebagai jenis proses.](/images/process-types.png)

**Dalam twelve-factor app, proses adalah warga negara kelas satu.** Proses dalam twelve-factor app mengambil isyarat kuat dari model [model proses unix untuk menjalankan daemon service](https://adam.herokuapp.com/past/2011/5/9/applying_the_unix_process_model_to_web_apps/). Menggunakan model ini, pengembang dapat merancang aplikasinya untuk menangani beban kerja yang beragam dengan menetapkan setiap jenis pekerjaan ke *jenis proses*. Misalnya, permintaan HTTP dapat ditangani oleh proses web, dan background tasks yang berjalan lama ditangani oleh worker process.

Ini tidak mengecualikan proses individu dari menangani multiplexing internalnya sendiri, melalui thread dalam VM runtime, atau model async/event yang ditemukan di tool seperti [EventMachine](https://github.com/eventmachine/eventmachine), [Twisted](http://twistedmatrix.com/trac/), atau [Node.js](http://nodejs.org/). Tetapi VM individu hanya dapat tumbuh sangat besar (skala vertikal) sehingga aplikasi juga harus dapat menjangkau beberapa proses yang berjalan pada beberapa mesin fisik.

Model proses benar-benar bersinar ketika tiba saatnya untuk scale out. [Sifat process twelve-factor app yang dapat dipartisi secara horizontal dan share-nothing](./processes) berarti bahwa menambahkan lebih banyak konkurensi adalah operasi yang sederhana dan andal. Array jenis proses dan jumlah proses dari setiap jenis dikenal sebagai *pembentukan proses*.

Proses twelve-factor app [tidak boleh melakukan daemonisasi](http://dustin.github.com/2010/02/28/running-processes.html) atau menulis file PID. Alih-alih mengandalkan process manager sistem operasi (seperti [systemd](https://www.freedesktop.org/wiki/Software/systemd/), process manager terdistribusi pada cloud platform, atau tool seperti [Foreman](http://blog.daviddollar.org/2011/05/06/introducing-foreman.html) dalam pengembangan) untuk mengelola [output streams](./logs), merespon proses yang crashed, dan menangani user-initiated restarts dan shutdown.
