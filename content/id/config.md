## III. Config
### Simpan konfigurasi di lingkungan

*Config* aplikasi adalah segala sesuatu yang kemungkinan besar akan bervariasi antara [deploys](./codebase) (staging, production, developer environments, dll). Ini termasuk:

* Resource menangani basis data, Memcached, dan [backing services](./backing-services) lainnya
* Kredensial untuk layanan eksternal seperti Amazon S3 atau Twitter
* Nilai Per-deploy seperti hostname canonical untuk deploy

Aplikasi terkadang menyimpan konfigurasi sebagai konstanta dalam kode. Ini merupakan pelanggaran twelve-factor, yang membutuhkan **pemisahan konfigurasi yang ketat dari kode**. Konfigurasi bervariasi secara substantial di seluruh deploys, kode tidak.

Pengujian lakmus untuk mengetahui apakah suatu aplikasi memiliki semua konfigurasi yang difaktorkan dengan benar dari kode adalah apakah basis kode dapat dijadikan open source kapan saja, tanpa mengorbankan kredensial apa pun.

Perhatikan bahwa definisi "config" ini **tidak** menyertakan konfigurasi internal aplikasi, seperti `config/routes.rb` pada Rails, atau bagaimana [modul kode dihubungkan](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/beans.html) dalam [Spring](http://spring.io/). Jenis konfigurasi ini tidak berbeda di anatar deploys, sehingga paling baik dilakukan dalam code.

Pendekatan lain untuk config adalah penggunaan file config yang tidak diperiksa ke revision control, seperti `config/database.yml` dalam Rails. Ini adalah peningkatan besar dari penggunaan konstanta yang diperiksa ke dalam repo kode, tetapi masih memiliki kelemahan: mudah untuk secara keliru memeriksa file konfigurasi ke repo; ada kecenderungan file konfigurasi tersebar di berbagai tempat dan format yang berbeda, sehingga sulit untuk melihat dan mengelola semua konfigurasi di satu tempat. Lebih lanjut, format ini cenderung khusus untuk bahas- atau spesifik pada suatu framework.

**Aplikasi twelve-factor menyimpan konfigurasi di *environment variables*** (sering disingkat menjadi *env vars* atau *env*). Env vars mudah untuk diubah di antara deploys tanpa mengubah kode apa pun; tidak seperti file config, kecil kemungkinan file tersebut diperiksa ke repo kode secara tidak sengaja; dan tidak seperti file konfigurasi custom, atau mekanisme konfigurasi lain seperti Java System Properties, mereka adalah standar bahasa- dan OS-agnostik.

Aspek lain dari manajemen konfigurasi adalah pengelompokan. Terkadang aplikasi mengelompokkan konfigurasi ke dalam grup bernama (sering disebut "environments") yang dinamai sesuai penerapa tertentu, seperti lingkungan `development`, `test`, dan `production` dalam Rails. Metode ini tidak menskalakan dengan rapi: karena semakin banyak deploys aplikasi yang dibuat, nama environment baru diperlukan, seperti `staging` atau `qa`. Saat proyek berkembang lebih jauh, pengembang dapat menambahkan environment khusus mereka densiri seperti `joes-staging`, yang mengakibatkan ledakan kombinatorial dari konfigurasi yang membuat pengelolaan deploys aplikasi menjadi sangat rapuh.

Dalam Twelve-factor app, env vars adalah kontrol granular, masing-masing ortogonal sepenuhnya ke env vars lainnya. Mereka tidak pernah dikelompokkan bersama sebagai "environments", tetapi dikelola secara independen untuk setiap deploy. Ini adalah model yang menaikkan skala dengan lancar karena aplikasi secara natural berkembang menjadi lebih banyak deploys selama masa pakainya.
