<h1> Daily Minimum Temperature</h1>
Nama : Raja Permata Boy <br>
NRP :05111740000070<br>
<h2>Business Understanding</h2>
Data yang digunakan pada workflow ini adalah data Daily Minimum Temperature, dimana data tersebut berisi data temperature minimal per hari, dari tahun 1981 sampai tahun 1990. Dari data ini, kita bisa mendapatkan berbagai informasi seperti rata-rata temperature minimum selama 1 bulan. <br>
<h2>Data Understanding</h2>
Data ini terdiri dari 2 kolom, yaitu:<br>
Date, merupakan variabel string yang berisi informasi waktu<br>
Daily minimum temperature, merupakan variabel floating point yang berisi informasi temperature minimal per hari<br>
<h2>Data Preparation</h2>
<img src="/Daily Minimum Temperature/img/dataprep.jpg"><br>
Pertama, kita akan menggunakan node File Reader untuk membaca Irish Energy Meter dataset.<br>
Berikut konfigurasi node file Reader :<br>
<img src="/Daily Minimum Temperature/img/filereader.jpg"><br>
Lalu, kita buat spark local context menggunakan node Create Local Big Data Environment<br>
Lalu kita jalankan componen Load Data yang terdiri dari beberapa node :<br>
<img src="/Daily Minimum Temperature/img/loaddata.jpg"><br>
Kita akan menambahkan 1 kolom lagi menggunakan node Row Id agar memiliki jumlah kolom yang sama dengan workflow tugas minggu 15.
Berikut konfigurasi node Row ID :
<img src="/Daily Minimum Temperature/img/rowid.jpg"><br>
Selanjutnya, data dipindahkan ke Spark Context dengan menggunakan node Hive To Spark<br>
Berikut adalah hasil tabel dari node Hive To Spark<br>
<img src="/Daily Minimum Temperature/img/hivetospark.jpg"><br>
<h2>Modelling</h2>
<img src="/Daily Minimum Temperature/img/modelling.jpg"><br>
Berikutnya adalah proses modelling<br>
Pertama-tama, kita akan menjalankan node extract time attributes. Berikut adalah isi dari node Extract Time Attributes:<br>
<img src="/Daily Minimum Temperature/img/extcomp.jpg"><br>
Ada 3 proses. Yang pertama adalah konversi date. Berikut konfigurasi Spark SQL Querynya :<br>
<img src="/Daily Minimum Temperature/img/sql1.jpg"><br>
Lalu, berikut adalah hasilnya :<br>
<img src="/Daily Minimum Temperature/img/sql1res.jpg"><br>
Dari proses sebelumnya didapatkan kolom eventDate, kita akan mengekstrak tahun, bulan, minggu, hari dengan Spark SQL Query yang kedua. Berikut konfigurasi SQL nya :<br>
<img src="/Daily Minimum Temperature/img/sql2.jpg"><br>
Berikut hasilnya:<br>
<img src="/Daily Minimum Temperature/img/sql2res.jpg"><br>
Selanjutnya, akan ada pengklasifikasian dari kolom dayOfWeek.Jika Saturday dan Sunday maka memiliki nilai 'WE', selain dari itu akan bernilai 'BD'. berikut adalah konfigurasi SQLnya:<br>
<img src="/Daily Minimum Temperature/img/sql3.jpg"><br>
Berikut hasilnya : <br>
<img src="/Daily Minimum Temperature/img/sql3res.jpg"><br>
Berikutnya akan masuk ke metanode Aggregation and time series. Berikut komponennya:<br>
<img src="/Daily Minimum Temperature/img/aggre1.jpg"><br>
<img src="/Daily Minimum Temperature/img/aggre2.jpg"><br>
Pertama, kita akan menginput data dan menyimpan di memory sementara menggunakan node Persist Spark Dataframe/RDD.<br>
Lalu, dari dataset tersebut akan dihitung rata-rata penggunaannya per segment(tahun,hari,bulan) menggunakan 3 node yaitu Spark GroupBy, Spark Pivot dan Spark Column Rename lalu semua akan digabungkan menggunakan Spark Joiner.
Yang pertama, akan menghitung usage keseluruhan dan rata-rata usage per tahun.
<img src="/Daily Minimum Temperature/img/byyear.jpg"><br>
Lalu akan menghitung rata-rata usage per bulan<br>
<img src="/Daily Minimum Temperature/img/bymonth.jpg"><br>
Lalu,akan menghitung rata-rata usage per minggu<br>
<img src="/Daily Minimum Temperature/img/byweek.jpg"><br>
Lalu, akan menghitung rata-rata usage per hari dalam 1 minggu<br>
<img src="/Daily Minimum Temperature/img/dayofweek.jpg"><br>
Lalu, akan menghitung rata-rata usage per harian<br>
<img src="/Daily Minimum Temperature/img/byday.jpg"><br>
Lalu, menghitung rata-rata usage per klasifikasi hari (WE atau BD)<br>
<img src="/Daily Minimum Temperature/img/dayclass.jpg"><br>
Lalu, semuanya akan digabung dengan menggunakan Spark Joiner. Lalu, hasilnya akan diteruskan ke node selanjutnya dalam workflow. Berikut hasil tablenya :<br>
<img src="/Daily Minimum Temperature/img/agreres.jpg"><br>
Selanjutnya, ada node Spark SQL Query. Kita akan menghitung persentase hari / minggu dan juga persentase segmen harian / hari. Berikut konfigurasinya :<br>
<img src="/Daily Minimum Temperature/img/sqlqueryconf.jpg"><br>
Hasilnya adalah sebagai berikut :<br>
<img src="/Daily Minimum Temperature/img/sqlqueryres.jpg"><br>
Karena terdapat missing value, maka kita akan menggunakan node Spark Missing Value untuk mengisi kekosongan dengan nilai 0.<br>
<img src="/Daily Minimum Temperature/img/miss1.jpg"><br>
Berikut hasilnya setelah menggunakan node Spark Missing Value<br>
<img src="/Daily Minimum Temperature/img/miss2.jpg"><br>
<h2>Evaluation</h2>
<img src="/Daily Minimum Temperature/img/evaluation.jpg"><br>
Berikut adalah komponen dari metanode PCA, K-Means, Scatter Plot<br>
<img src="/Daily Minimum Temperature/img/pcacomp.jpg"><br>
Pada node Spark normalizer semua data kecuali ID, dinormalisasi menjadi range 0 - 1. Pada node setelah Denormalizer data dioutputkan menjadi 2 bentuk yaitu visualisasi dan data table yang diteruskan ke general workflow. Selanjutnya, data output tadi dimasukkan kembali ke Local Big Data Environment menggunakan 2 node, yaitu Spark to Hive untuk load menjadi Apache Hive dan Spark to Parquet untuk load menjadi HDFS.
Hasilnya adalah sebagai berikut :<br>
<img src="/Daily Minimum Temperature/img/evres1.jpg"><br>
<img src="/Daily Minimum Temperature/img/evres2.jpg"><br>

<h2>Deployment</h2>
<img src="/Daily Minimum Temperature/img/deployment.jpg"><br>
Pada tahap ini dilakukan perubahan data dari spark kembali mejadi hive serta menyimpan spark kedalam HDFS dalam bentuk parquet, hasil dari data tersebut adalah sebagai berikut<br>
<img src="/Daily Minimum Temperature/img/deploy.jpg"><br>
