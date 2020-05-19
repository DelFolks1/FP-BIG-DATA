<h1>Sales of Shampoo</h1>
Nama : Raja Permata Boy <br>
NRP : 05111740000070 <br>

<h2>Business Understanding</h2>
Dataset yang digunakan adalah Sales of Shampoo over 3 years<br>
Dari dataset ini kita bisa mendapat informasi berupa berapa jumlah sampo yang terjual tiap bulannya atau tiap tahunnya<br>
<h2>Data Understanding</h2>
Data ini terdiri dari 2 kolom, yaitu:<br>
- Month, merupakan variabel string yang berisi informasi waktu berupa tanggal dan bulan<br>
- Sales of a Shampoo over three year period, merupakan variabel floating point yang berisi angka penjualan shampoo per bulan.<br>
<h2>Data Preparation</h2>
<img src="/Sales of Shampoo/img/dataprep.jpg"><br>
Pertama-tama dataset akan dibaca dengan menggunakan node File Reader. Juga akan dibuat Spark Local Context menggunakan Create Local Big Data Environment
Berikut konfigurasinya :<br>
<img src="/Sales of Shampoo/img/filereader.jpg"><br>
Berikutnya, ada metanode Load Data yang terdiri dari :
<img src="/Sales of Shampoo/img/loaddata.jpg"><br>
Kita akan menambahkan 1 kolom lagi menggunakan node Row Id agar memiliki jumlah kolom yang sama dengan workflow dataset yang Monthly Beer Production agar cepat selesai.
Berikut konfigurasi node Row ID :<br>
<img src="/Sales of Shampoo/img/rowid.jpg"><br>
Kita memberi nama yang sama row ID yang sama dengan dataset Beer.<br>
Lalu, kita melakukan pembuatan table pada hive dan setelah itu melakukan load table yang telah dibuat, hasil table yang telah di buat adalah sebagai berikut.<br>
<img src="/Sales of Shampoo/img/hivetospark.jpg"><br>
<h2>Modelling</h2>
<img src="/Sales of Shampoo/img/modelling.jpg"><br>
Berikutnya adalah proses modelling Pertama-tama, kita akan menjalankan node extract time attributes. Berikut adalah isi dari node Extract Time Attributes:<br>
<img src="/Sales of Shampoo/img/extcomp.jpg"><br>
Ada 3 proses. Yang pertama adalah konversi date. Berikut konfigurasi Spark SQL Querynya :<br>
<img src="/Sales of Shampoo/img/konversi.jpg"><br>
Lalu, berikut adalah hasilnya :<br>
<img src="/Sales of Shampoo/img/konvres.jpg"><br>
Dari proses sebelumnya didapatkan kolom eventDate, kita akan mengekstrak tahun, bulan 
dengan Spark SQL Query yang kedua. Berikut konfigurasi SQL nya :<br>
<img src="/Sales of Shampoo/img/ekstraksi.jpg"><br>
Hasilnya adalah sebagai berikut : <br>
<img src="/Sales of Shampoo/img/ekstraksi2.jpg"><br>
Selanjutnya, akan ada pengklasifikasian secara musim.Saya membaginya menjadi 4 musim. 
Berikut adalah konfigurasi SQLnya:<br>
<img src="/Sales of Shampoo/img/musim1.jpg"><br>
Hasilnya adalah sebagai berikut : <br>
<img src="/Sales of Shampoo/img/musim2.jpg"><br>
Berikutnya akan masuk ke metanode Aggregation and time series. Berikut komponennya:<br>
<img src="/Sales of Shampoo/img/agre.jpg"><br>
Menghitung usage keseluruhan dan menghitung rata-rata per segment tahun.<br>
Menghitung rata-rata per segment bulan.<br>
Menghitung rata-rata per segment bulanan dalam 1 tahun.<br>
Menghitung rata-rata per month classifier(musim)<br>
Setelah itu, data rata-rata tersebut dijoin menggunakan node Spark Joiner dan diteruskan ke general workflow.<br>
Hasil akhir dari table yang telah selesai di proses adalah sebagai berikut:<br>
<img src="/Sales of Shampoo/img/rdd.jpg"><br>
Selanjutnya, ada node Spark SQL Query. Kita akan menghitung persentase month classifier/monthly. Berikut konfigurasinya :<br>
<img src="/Sales of Shampoo/img/sqlquery.jpg"><br>
Hasilnya adalah sebagai berikut :<br>
<img src="/Sales of Shampoo/img/sqlquery2.jpg"><br>
Karena terdapat missing value, maka kita akan menggunakan node Spark Missing Value untuk mengisi kekosongan dengan nilai 0.
Berikut hasilnya setelah menggunakan node Spark Missing Value
Hasil :<br>
<img src="/Sales of Shampoo/img/miss.jpg"><br>

<h2>Evaluation</h2>
<img src="/Sales of Shampoo/img/pca.jpg"><br>
Berikut adalah komponen dari metanode diatas :<br>
<img src="/Sales of Shampoo/img/pcacomp.jpg"><br>
Pada node Spark normalizer semua data kecuali ID, dinormalisasi menjadi range 0 - 1.<br>
Pada node setelah Denormalizer data dioutputkan menjadi 2 bentuk yaitu visualisasi dan data table yang diteruskan ke general workflow. <br>
Selanjutnya, data output tadi dimasukkan kembali ke Local Big Data Environment menggunakan 2 node, yaitu Spark to Hive untuk load menjadi Apache Hive dan Spark to Parquet untuk load menjadi HDFS. <br>
Hasilnya adalah sebagai berikut :<br>
<img src="/Sales of Shampoo/img/kmeans.jpg"><br>
<img src="/Sales of Shampoo/img/scar.jpg"><br>
<h2>Deployment</h2>
<img src="/Sales of Shampoo/img/deployment.jpg"><br>
Pada tahap ini dilakukan perubahan data dari spark kembali mejadi hive serta menyimpan spark kedalam HDFS dalam bentuk parquet, hasil dari data tersebut adalah sebagai berikut<br>
<img src="/Sales of Shampoo/img/deploy.jpg"><br>

