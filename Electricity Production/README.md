
<h1> Electricity Production</h1>
Nama : Raja Permata Boy Mangatur Simarmata <br>
NRP : 05111740000070 <br>

<h2>Business Understanding</h2>
Dataset yang bisa digunakan dapat dilakukan proses clusterisasi yang bertujuan memberikan kita informasi yang berguna. Dataset yang digunakan adalah data produksi peralatan elektrik untuk pabrik industri per hari dari tahun 1985 hingga awal tahun 2018. <br>
<h2>Data Understanding</h2>
Dataset ini memiliki 2 kolom : <br>
- Date, merupakan informasi waktu <br>
- IPG2211A2N, merupakan variabel floating point yang berisi informasi nilai produksi pada Date yang bersesuaian<br>
<h2>Data Preparation</h2>
<img src="/Electricity Production/img/dataprep.jpg"><br>
Pertama-tama dataset akan dibaca dengan menggunakan node File Reader. Juga akan dibuat Spark Local Context menggunakan Create Local Big Data Environment<br>
Berikut konfigurasinya : <br>
<img src="/Electricity Production/img/filereader.jpg"><br>
Berikutnya, ada metanode Load Data yang terdiri dari :<br>
<img src="/Electricity Production/img/loaddata.jpg"><br>
Kita akan menambahkan 1 kolom lagi menggunakan node Row Id agar memiliki jumlah kolom yang sama dengan workflow tugas minggu 15. <br>
Berikut konfigurasi node Row ID : <br>
<img src="/Electricity Production/img/rowid.jpg"><br>
Selanjutnya, data dipindahkan ke Spark Context dengan menggunakan node Hive To Spark<br>
Berikut adalah hasil tabel dari node Hive To Spark <br>
<img src="/Electricity Production/img/hivetospark.jpg"><br>
<h2>Modelling</h2>
<img src="/Electricity Production/img/modelling.jpg"><br>
Berikutnya adalah proses modelling<br>
Pertama-tama, kita akan menjalankan node extract time attributes. Berikut adalah isi dari node Extract Time Attributes: <br>
<img src="/Electricity Production/img/extracttime.jpg"><br>
Ada 3 proses. Yang pertama adalah konversi date. Berikut konfigurasi Spark SQL Querynya :
<img src="/Electricity Production/img/konversiconf.jpg"><br>
Lalu, berikut adalah hasilnya :<br>
<img src="/Electricity Production/img/konversires.jpg"><br>
Dari proses sebelumnya didapatkan kolom eventDate, kita akan mengekstrak tahun, bulan, minggu, hari  dengan Spark SQL Query yang kedua. Berikut konfigurasi SQL nya :<br>
<img src="/Electricity Production/img/extconf.jpg"><br>
Berikut hasil ekstraksinya :
<img src="/Electricity Production/img/extres.jpg"><br>
Selanjutnya, akan ada pengklasifikasian dari kolom dayOfWeek.Jika Saturday dan Sunday maka memiliki nilai 'WE', selain dari itu akan bernilai 'BD'. berikut adalah konfigurasi SQLnya:<br>
<img src="/Electricity Production/img/classconf.jpg"><br>
Berikut hasilnya:<br>
<img src="/Electricity Production/img/classres.jpg"><br>
Berikutnya akan masuk ke metanode Aggregation and time series. Berikut komponennya:<br>
<img src="/Electricity Production/img/aggre1.jpg"><br>
<img src="/Electricity Production/img/aggre2.jpg"><br>
<h2>Evaluation</h2>
<h2>Deployment</h2>
