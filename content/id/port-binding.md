## VII. Port binding
### Ekspor layanan melalui port binding

Aplikasi web terkadang dijalankan dalam container webserver. Contohnya, aplikasi PHP dapat dijalankan sebagai modul dalam [Apache HTTPD](http://httpd.apache.org/), atau aplikasi Java mungkin dijalankan dalam [Tomcat](http://tomcat.apache.org/).

**The twelve-factor app benar-benar bersifat self-contained** dan tidak bergantung pada injeksi runtime webserver ke dalam lingkungan eksekusi untuk membuat layanan web-facing. Aplikasi web **mengekspor HTTP sebagai layanan dengan mengikat ke port**, dan mendengarkan request yang masuk di port tersebut.

Dalam lingkungan pengembanga lokal, pengembang mengunjungi URL layanan seperti `http://localhost:5000/` untuk mengakses layanan yang diekspor oleh aplikasi. Dalam deployment, lapisan routing menangani permintaan routing dari namahost public ke proses web yang terikat port.

Ini biasanya diimplementasikan dengan menggunakan [deklarasi dependensi](./dependencies) untuk menambahkan library server web ke apliaksi, seperti [Tornado](http://www.tornadoweb.org/) untuk Python, [Thin](http://code.macournoyer.com/thin/) untuk Ruby, atau [Jetty](http://www.eclipse.org/jetty/) untuk Java dan bahasa berbasis JVM lainnya. Ini sepenuhnya terjadi pada *ruang pengguna*, yaitu, di dalam kode aplikasi. Kontrak dengan lingkungan eksekusi mengikat port untuk melayani permintaan.

HTTP bukan satu-satunya layanan yang dapat diekspor dengan port binding. Hampir semua jenis perangkat lunak server dapat dijalankan melalui proses mengikat ke port dan menunggu permintaan masuk. Contohnya termasuk [ejabberd](http://www.ejabberd.im/) (mengenai [XMPP](http://xmpp.org/)), dan [Redis](http://redis.io/) (mengenai [Redis protocol](http://redis.io/topics/protocol)).

Perhatikan juga bahwa pendekatan port-binding berarti satu aplikasi bisa menjadi [backing service](./backing-services) untuk aplikasi lain, dengan memberikan URL ke aplikasi cadangan sebagai resource handle dalam [config](./config) untuk aplikasi konsumsi.
