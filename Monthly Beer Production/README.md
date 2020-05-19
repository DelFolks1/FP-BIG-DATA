
<h1>Monthly Beer Production in Australia</h1>
Nama:Raja Permata Boy<br>
NRP : 05111740000070<br>

<h2>Business Understanding</h2>
Dataset ini adalah dataset yang berisi jumlah produksi Bir di Australia per bulannya. Dari dataset ini, kita bisa mendapatkan berbagai informasi seperti rata-rata bir yang diproduksi perbulan.<br>

<h2>Data Understanding</h2>
Data ini terdiri dari 2 kolom, yaitu:<br>
Month, merupakan variabel string yang berisi informasi waktu berupa bulan dan tahun<br>
Monthly Beer Production, merupakan variabel  yang berisi informasi produksi bir tiap bulannya<br>

<h2>Data Preparation</h2>
<img src="/Monthly Beer Production/img/dataprep.jpg"><br>
Pertama-tama dataset akan dibaca dengan menggunakan node File Reader. Juga akan dibuat Spark Local Context menggunakan Create Local Big Data Environment
Berikut konfigurasinya :<br>
<img src="/Monthly Beer Production/img/filereader.jpg"><br>
Berikutnya, ada metanode Load Data yang terdiri dari :<br>
<img src="/Monthly Beer Production/img/loaddata.jpg"><br>
Kita akan menambahkan 1 kolom lagi menggunakan node Row Id agar memiliki jumlah kolom yang sama dengan workflow tugas minggu 15.
Berikut konfigurasi node Row ID :<br>
<img src="/Monthly Beer Production/img/rowid.jpg"><br>
Selanjutnya, data dipindahkan ke Spark Context dengan menggunakan node Hive To Spark
Berikut adalah hasil tabel dari node Hive To Spark<br>
<img src="/Monthly Beer Production/img/hivetospark.jpg"><br>
<h2>Modelling</h2>
<img src="/Monthly Beer Production/img/modelling.jpg"><br>
Berikutnya adalah proses modelling
Pertama-tama, kita akan menjalankan node extract time attributes. Berikut adalah isi dari node Extract Time Attributes:<br>
<img src="/Monthly Beer Production/img/extcomp.jpg"><br>
Ada 3 proses. Yang pertama adalah konversi date. Berikut konfigurasi Spark SQL Querynya :
<img src="/Monthly Beer Production/img/konversi.jpg"><br>
Lalu, berikut adalah hasilnya :
<img src="/Monthly Beer Production/img/konvres.jpg"><br>
Dari proses sebelumnya didapatkan kolom eventDate, kita akan mengekstrak tahun, bulan, minggu, hari dengan Spark SQL Query yang kedua. Berikut konfigurasi SQL nya :<br>
<img src="/Monthly Beer Production/img/ekstraksi.jpg"><br>
Berikut adalah hasilnya : <br>
<img src="/Monthly Beer Production/img/hasil.jpg"><br>
Selanjutnya, akan ada pengklasifikasian secara musim.Karena Australia memiliki 4 musim maka saya membaginya menjadi 4 musim tersebut. berikut adalah konfigurasi SQLnya:<br>
<img src="/Monthly Beer Production/img/class.jpg"><br>
Hasilnya sebagai berikut : <br>
<img src="/Monthly Beer Production/img/classres.jpg"><br>
Berikutnya akan masuk ke metanode Aggregation and time series. Berikut komponennya:<br>
<img src="/Monthly Beer Production/img/agr.jpg"><br>
Menghitung usage keseluruhan dan menghitung rata-rata per segment tahun.<br>
Menghitung rata-rata per segment bulan.<br>
Menghitung rata-rata per segment bulanan dalam 1 tahun.<br>
Menghitung rata-rata per month classifier(musim)<br>
Setelah itu, data rata-rata tersebut dijoin menggunakan node Spark Joiner dan diteruskan ke general workflow.<br>
Hasil akhir dari table yang telah selesai di proses adalah sebagai berikut:<br>
<img src="/Monthly Beer Production/img/res.jpg"><br>
Selanjutnya, ada node Spark SQL Query. Kita akan menghitung persentase month classifier/monthly. Berikut konfigurasinya :<br>
<img src="/Monthly Beer Production/img/sql1.jpg"><br>
Hasilnya adalah sebagai berikut :<br>
<img src="/Monthly Beer Production/img/sql2.jpg"><br>
Karena terdapat missing value, maka kita akan menggunakan node Spark Missing Value untuk mengisi kekosongan dengan nilai 0.<br>
Berikut hasilnya setelah menggunakan node Spark Missing Value<br>
Hasil : <br>
<img src="/Monthly Beer Production/img/miss.jpg"><br>
<h2>Evaluation</h2>
<img src="/Monthly Beer Production/img/evaluation.jpg"><br>
Berikut adalah komponen dari metanode diatas :<br>
<img src="/Monthly Beer Production/img/pcacomp.jpg"><br>
Pada node Spark normalizer semua data kecuali ID, dinormalisasi menjadi range 0 - 1. Pada node setelah Denormalizer data dioutputkan menjadi 2 bentuk yaitu visualisasi dan data table yang diteruskan ke general workflow. Selanjutnya, data output tadi dimasukkan kembali ke Local Big Data Environment menggunakan 2 node, yaitu Spark to Hive untuk load menjadi Apache Hive dan Spark to Parquet untuk load menjadi HDFS.
Hasilnya adalah sebagai berikut :<br>
<img src="/Monthly Beer Production/img/evres1.jpg"><br>
<img src="/Monthly Beer Production/img/evres2.jpg"><br>
<h2>Deployment</h2>
<img src="/Monthly Beer Production/img/deployment.jpg"><br>
Pada tahap ini dilakukan perubahan data dari spark kembali mejadi hive serta menyimpan spark kedalam HDFS dalam bentuk parquet, hasil dari data tersebut adalah sebagai berikut<br>
<img src="/Monthly Beer Production/img/last.jpg"><br>
