
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
<img src="/Electricity Production/img/aggre1.jpg">
<img src="/Electricity Production/img/aggre2.jpg"><br>
Pertama, kita akan menginput data dan menyimpan di memory sementara menggunakan node Persist Spark Dataframe/RDD.<br>
Lalu, dari dataset tersebut akan dihitung rata-rata penggunaannya per segment(tahun,hari,bulan, dan jam) menggunakan 3 node yaitu Spark GroupBy, Spark Pivot dan Spark Column Rename lalu semua akan digabungkan menggunakan Spark Joiner.
Yang pertama, akan menghitung usage keseluruhan dan rata-rata usage per tahun.<br>
<img src="/Electricity Production/img/byyear.jpg"><br>
Lalu akan menghitung rata-rata usage per bulan<br>
<img src="/Electricity Production/img/bymonth.jpg"><br>
Lalu,akan menghitung rata-rata usage per minggu<br>
<img src="/Electricity Production/img/byweek.jpg"><br>
Lalu, akan menghitung rata-rata usage per harian
<img src="/Electricity Production/img/byday.jpg"><br>
Lalu, menghitung rata-rata usage per klasifikasi hari (WE atau BD)<br>
<img src="/Electricity Production/img/dayclass.jpg"><br>
Lalu, semuanya akan digabung dengan menggunakan Spark Joiner. Lalu, hasilnya akan diteruskan ke node selanjutnya dalam workflow. Berikut hasil tablenya :<br>
<img src="/Electricity Production/img/rdd.jpg"><br>
Selanjutnya, ada node Spark SQL Query. Kita akan menghitung persentase hari / minggu dan juga persentase segmen harian / hari. Berikut konfigurasinya :<br> 
<img src="/Electricity Production/img/sparksql.jpg"><br>
Hasilnya adalah sebagai berikut : <br>
<img src="/Electricity Production/img/sparksqlres.jpg"><br>
Karena terdapat missing value, maka kita akan menggunakan node Spark Missing Value untuk mengisi kekosongan dengan nilai 0. <br>
<img src="/Electricity Production/img/miss.jpg"><br>
Berikut hasilnya setelah menggunakan node Spark Missing Value <br>
<img src="/Electricity Production/img/miss2.jpg"><br>
<h2>Evaluation</h2>
<img src="/Electricity Production/img/evaluation.jpg"><br>
Berikut adalah komponen dari metanode diatas :
<img src="/Electricity Production/img/pca.jpg"><br>
Pada node Spark normalizer semua data kecuali ID, dinormalisasi menjadi range 0 - 1. Pada node setelah Denormalizer data dioutputkan menjadi 2 bentuk yaitu visualisasi dan data table yang diteruskan ke general workflow. Selanjutnya, data output tadi dimasukkan kembali ke Local Big Data Environment menggunakan 2 node, yaitu Spark to Hive untuk load menjadi Apache Hive dan Spark to Parquet untuk load menjadi HDFS.<br>
Hasilnya adalah sebagai berikut : <br>
<img src="/Electricity Production/img/pcaview.jpg"><br>
<img src="/Electricity Production/img/pcaview2.jpg"><br>
<h2>Deployment</h2>
<img src="/Electricity Production/img/deployment.jpg"><br>
Pada tahap ini dilakukan perubahan data dari spark kembali mejadi hive serta menyimpan spark kedalam HDFS dalam bentuk parquet, hasil dari data tersebut adalah sebagai berikut<br>
<img src="/Electricity Production/img/parquet.jpg"><br>
