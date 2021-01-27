## I. Basis Kode
### Satu basis kode dilacak dalam kontrol revisi, untuk deployment yang banyak

Aplikasi twelve-factor selalu dilacak dalam sistem kontrol, seperti [Git](http://git-scm.com/), [Mercurial](https://www.mercurial-scm.org/), atau [Subversion](http://subversion.apache.org/).  Salinan basis data pelacakan revisi dikenal sebagai *repositori kode*, sering kali disingkat menjadi *repo kode* atau hanya *repo*.

Sebuah *basis kode* adalah repo tunggal (dalam sistem kontrol revisi terpusat seperti Subversion), atau kumpulan repo apa pun yang berbagi komitmen root (dalam sistem kontrol revisi terdesentralisasi seperti Git).

![Satu basis kode memetakan deployment yang banyak](/images/codebase-deploys.png)

Selalu ada korelasi satu-ke-satu antara basis kode dan aplikasi:

* Jika ada beberapa basis kode, ini bukan aplikasi -- ini adalah sistem terdistribusi. Setiap komponen dalam sistem terdistribusi adalah sebuah aplikasi, dan masing-masing dapat mematuhi twelve-factor.
* Beberapa aplikasi yang berbagi kode yang sama merupakan pelanggaran twelve-factor. Solusinya di sini adalah untuk memfaktorkan kode bersama ke dalam pustaka yang dapat disertakan melalui [dependency manager](./dependencies).

Hanya ada satu basis kode per aplikasi, tetapi akan ada banyak deployment aplikasi. Sebuah *deploy* adalah instance aplikasi yang sedang berjalan. Ini biasanya merupakan situs dalam tahap produksi, dan satu atau beberapa situs dalam tahap staging. Selain itu, setiap pengembang memiliki salinan aplikasi yang berjalan di lingkungan pengembangan lokal mereka, yang masing-masing juga memenuhi syarat deployment.

Basis kode sama di semua deployment, meskipun versi yang berbeda mungkin aktif di setiap deployment. Misalnya, pengembang memiliki beberapa komit yang belum diterapkan pada tahap staging; staging memiliki beberapa komit yang belum diterapkan pada tahap produksi. Tetapi semuanya berbagi basis kode yang sama, sehingga membuatnya dapat diidentifikasi sebagai deployment yang berbeda dari aplikasi yang sama.

