## V. Build, release, run
### Pisahkan tahapan build dan run secara ketat

Sebuah [basis kode](./codebase) diubah menjadi (non-development) deploy melalui tiga tahap:

* *Tahapan build* adalah transformasi yang mengubah repo kode menjadi bundel yang dapat dieksekusi yang dikenal sebagai *build*. Menggunakan versi kode pada commit yang ditentukan oleh proses deployment, tahap build mengambil vendor [dependencies](./dependencies) dan mengompilasi biner dan aset.
* *Tahap rilis* mengambil build yang dihasilkan oleh tahap build dan menggabungkannya dengan [config](./config) pada deploy saat ini. *Rilis* yang dihasilkan berisi build dan config dan siap untuk dieksekusi langsung di lingkungan eksekusi.
* *Tahap run* (juga dikenal sebagai "runtime") menjalankan aplikasi dalam lingkungan eksekusi, dengan meluncurkan beberapa set [proses](./processes) aplikasi terhadap rilis yang dipilih.

![Kode menjadi sebuah build, yang digabungkan dengan config untuk membuat rilis.](/images/release.png)

**Twelve-factor app menggunakan pemisahan ketat antara tahap build, rilis, dan run.** Misalnya, tidak mungkin membuat perubahan pada kode saat runtime karena tidak ada cara untuk menyebarkan perubahan tersebut pada tahap build.

Tool deployment biasanya menawarkan tool manajemen rilis, terutama kemampuan untuk memutar kembali ke rilis sebelumnya. Sebagai contoh, tool deployment [Capistrano](https://github.com/capistrano/capistrano/wiki) menyimpan rilis di subdirektori bernama `rilis`, dimana rilis saat ini adalah symlink ke direktori rilis saat ini. Perintah `rollback` memudahkan untuk memutar kembali dengan cepat ke rilis sebelumnya.

Setiap rilis seharusnya memiliki ID rilis unik, seperti timestamp rilis (seperti `2011-04-06-20:32:17`) atau angka yang bertambah (seperti `v100`). Rilis adalah buku besar khusus append dan rilis tidak dapat dimutasi setelah dibuat. Setiap perubahan harus membuat rilis baru.

Build dimulai oleh pengembang aplikasi setiap kali kode baru di-deploy. Eksekusi runtime, sebaliknya, dapat terjadi secara otomatis dalam kasus-kasus seperti reboot server atau proses crashed yang di-restart oleh process manager. Oleh karena itu, tahap run harus dijaga sesedikit mungkin karena masalah yang mencegah aplikasi berjalan dapat menyebabkannya rusak di tengah malam saat tidak ada pengembang yang siap. Tahap build bisa jadi lebih kompleks karena error selalu ada di depan latar untuk pengembang yang melakukan deployment aplikasi.

