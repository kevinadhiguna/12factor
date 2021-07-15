## X. Dev/prod parity
### Jaga agar tahap development, staging, dan produksi semirip mungkin

Secara historis, ada kesenjangan substantial antara tahap development (pengembang membuat pengeditan langsung ke [deploy](./codebase) lokal aplikasi) dan tahap production (aplikasi yang di-deploy yang diakses oleh end user). Kesenjangan ini terwujud dalam tiga bidang:

* **Time gap**: Pengembang mungkin mengerjakan code yang membutuhkan waktu berhari-hari, berminggu-minggu, atau bahkan berbulan-bulan untuk masuk ke tahap production.
* **Personnel Gap**: Pengembang menulis code, ops engineers melakukan deployment aplikasi tersebut.
* **Tools Gap**: Pengembang mungkin menggunakan stack seperti Nginx, SQLite, dan OS X, sedangkan deployment tahap production menggunakan Apache, MySQL, dan Linux.

**Twelve-factor app dirancang untuk [continuous deployment](http://avc.com/2011/02/continuous-deployment/) dengan menjaga gap antara tahap pengembangan dan production tetap kecil.** Melihat tiga gap yang dijelaskan di atas:

* Buat time gap kecil: pengembang dapat menulis code dan men-deploynya beberapa jam atau bahkan beberapa menit kemudian.
* Buat personnel gap kecil: pengembang yang menulis code sangat terlibat dalam deployment dan mengamati perilakunya dalam production.
* Buat tools gap kecil: pertahankan agar tahap pengembangan dan produksi semirip mungkin.

Meringkas hal di atas menjadi sebuah tabel:

<table>
  <tr>
    <th></th>
    <th>Aplikasi Tradisional</th>
    <th>Aplikasi Twelve-factor</th>
  </tr>
  <tr>
    <th>Waktu antara deployment</th>
    <td>Minggu</td>
    <td>Jam</td>
  </tr>
  <tr>
    <th>Penulis Code vs Deployer Code</th>
    <td>Orang yang berbeda</td>
    <td>Orang yang sama</td>
  </tr>
  <tr>
    <th>Dev vs Lingkungan Produksi</th>
    <td>Berbeda</td>
    <td>Semirip mungkin</td>
  </tr>
</table>

[Backing services](./backing-services), seperti basis data aplikasi, queueing system, atau cache, adalah salah satu area dimana dev/prod merupakan hal yang penting. Banyak bahasa menawarkan library yang menyederhanakan akses ke backing service, termasuk *adapters* untuk berbagai jenis layanan. Beberapa contohnya ada pada tabel di bawah ini.

<table>
  <tr>
    <th>Tipe</th>
    <th>Bahasa</th>
    <th>Library</th>
    <th>Adapters</th>
  </tr>
  <tr>
    <td>Basis Data</td>
    <td>Ruby/Rails</td>
    <td>ActiveRecord</td>
    <td>MySQL, PostgreSQL, SQLite</td>
  </tr>
  <tr>
    <td>Queue</td>
    <td>Python/Django</td>
    <td>Celery</td>
    <td>RabbitMQ, Beanstalkd, Redis</td>
  </tr>
  <tr>
    <td>Cache</td>
    <td>Ruby/Rails</td>
    <td>ActiveSupport::Cache</td>
    <td>Memory, filesystem, Memcached</td>
  </tr>
</table>

Pengembang terkadang menemukan daya tarik yang bagus dalam menggunakan backing service ringan di lingkungan lokal mereka, sementara backing service yang lebih serius dan kuat akan digunakan dalam produksi. Contohnya, menggunakan SQLite secara lokal dan PostgreSQL dalam tahap production; atau memori proses lokal untuk cache dalam pengembangan dan Memcached dalam tahap production.

**Pengembang twelve-factor menahan keinginan untuk menggunakan backing services berbeda antara tahap pengembangan dan production**, bahkan ketika adapters secara teoritis mengabstraksi perbedaan apa pun dalam backing services. Pebedaan antara backing services berarti bahwa ketidakcocokan kecil muncul, menyebabkan code yang berfungsi dan lulus test dalam tahap pengembangan atau staging gagal dalam tahap production. Jenis error ini menciptakan gesekan yang yang menghalangi continuous deployment. Biaya gesekan ini dan peredam berikutnya dari continuous deployment sangat tinggi jika dipertimbangkan secara keseluruhan aplikasi selama masa pakai aplikasi.

Layanan lokal yang ringan kurang menarik dibandingkan sebelumnya. Backing services modern seperti Memcached, PostgreSQL, dan RabbitMQ tidak sulit untuk diinstall dan dijalankan berkat packaging system modern, seperti [Homebrew](http://mxcl.github.com/homebrew/) dan [apt-get](https://help.ubuntu.com/community/AptGet/Howto). Secara alternatif, declarative provisioning tools seperti [Chef](http://www.opscode.com/chef/) dan [Puppet](http://docs.puppetlabs.com/) dikombinasikan dengan lingkungan virtual ringan seperti [Docker](https://www.docker.com/) dan [Vagrant](http://vagrantup.com/) memungkinkan pengembang untuk menjalankan lingkungan lokal yang mendekati lingkungan production. Biaya instalasi dan penggunaan sistem ini rendah dibandingkan dengan manfaat dev/prod parity dan continuous deployment.

Adapters ke backing services berbeda masih berguna karena mereka membuat porting ke backing services baru relatif tidak merepotkan. Tetapi smeua deployment aplikasi (lingkungan pengembang, tahap staging, tahap production) harus menggunakan jenis dan versi yang sama dengan setiap backing services.
