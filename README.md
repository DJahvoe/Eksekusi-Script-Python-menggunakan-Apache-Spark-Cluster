# **Eksekusi Script Python menggunakan Apache Spark Cluster**

## **Setup**

<hr>

1. Membuat folder baru, dan di dalam folder tersebut membuat file konfigurasi dalam format **.yml** (dalam contoh ini adalah **docker-compose.yml**) <br>
2. Membuat konfigurasi yang diinginkan di dalam file **.yml** (berikut contoh konfigurasi dengan 5 worker) <br>

```
version: "2"

services:
  spark:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=master
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no
    ports:
      - "8080:8080"
  spark-worker-1:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark:7077
      - SPARK_WORKER_MEMORY=1G
      - SPARK_WORKER_CORES=1
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no
  spark-worker-2:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark:7077
      - SPARK_WORKER_MEMORY=1G
      - SPARK_WORKER_CORES=1
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no
  spark-worker-3:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark:7077
      - SPARK_WORKER_MEMORY=1G
      - SPARK_WORKER_CORES=1
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no
  spark-worker-4:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark:7077
      - SPARK_WORKER_MEMORY=1G
      - SPARK_WORKER_CORES=1
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no
  spark-worker-5:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark:7077
      - SPARK_WORKER_MEMORY=1G
      - SPARK_WORKER_CORES=1
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no
```

Keterangan: <br>

- **spark-worker-\*** menandakan jumlah node slave yang diinginkan untuk mengeksekusi script yang nantinya akan memberikan hasil ke node master. Untuk dilakukan penambahan node slave, hanya perlu menduplikasikan konfigurasi berikut

```
  spark-worker-x:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark:7077
      - SPARK_WORKER_MEMORY=1G
      - SPARK_WORKER_CORES=1
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no
```

- **SPARK_MODE** menunjukkan node yang dikonfigurasi bertindak sebagai mode yang di-set.
- **SPARK_MASTER_URL** menunjukkan url dari node master tempat dimana hasil eksekusi slave dikembalikan.
- **SPARK_WORKER_MEMORY** merupakan banyaknya memory yang digunakan tiap worker/slave.
- **SPARK_WORKER_CORES** digunakan untuk mengkonfigurasi berapa banyak _core_ yang akan digunakan tiap worker/slave.
- **SPARK_RPC_AUTHENTICATION_ENABLED** menunjukkan apakah digunakannya autentifikasi dalam protocol untuk berkomunikasi antar spark process.
- **SPARK_RPC_ENCRYPTION_ENABLED** digunakan untuk menyalakan atau mematikan enkripsi dalam komunikasi protocol.
- **SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED** digunakan untuk menyalakan atau mematikan enkripsi di dalam tempat penyimpanan local.
- **SPARK_SSL_ENABLED** digunakan untuk menyalakan atau mematikan security di dalam transport layer.

3. Setelah konfigurasi selesai:

- konfigurasi dapat dijalankan dengan menggunakan perintah berikut <br>
  `docker-compose -f [nama-file] up -d` <br>
  agar inisialisasi terjadi di background-process
- sedangkan untuk menghentikannya dapat menggunakan <br> `docker-compose down`

4. Apabila pembuatan container berhasil, maka akan keluar hasil sebagai berikut <br>
   ![inisialisasi.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/inisialisasi.jpg)

5. Untuk memastikan container telah terbuat, cek dengan perintah `docker ps` <br>
   ![docker_ps.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/docker_ps.jpg)

6. Pengecekan juga dapat dilakukan melalui web UI http://localhost:8080 <br>
   ![localhost.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/localhost.jpg)

## **Eksekusi script**

<hr>

1. Pengecekan container yang tersedia dengan `docker ps` <br>
   ![docker_ps.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/docker_ps.jpg)

2. Memasukkan container id untuk mengeksekusi bash di dalam container dengan format perintah `docker exec -it [container_id] [direktori_bash]` <br>
   ![container_id.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/container_id.jpg)

3. Submit job ditujukan kepada worker menggunakan perintah **spark-submit** dengan format berikut `spark-submit --master [alamat_master_node] [direktori_script] [banyak_partisi]` dengan contoh **alamat_master_node** sebagai berikut<br>
   `spark-submit --master spark://ada6c9c0effc:7077 examples/src/main/python/pi.py 10` <br>
   ![spark_submit_1.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/spark_submit_1.jpg)<br>
4. Apabila proses berhasil dijalankan, akan muncul hasil sebagai berikut <br>
   ![hasil_spark_submit.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/hasil_spark_submit.jpg)<br>

## **Percobaan**

<hr>

### **TASK**

- Percobaan 1 (2 Workers, 2 Cores, 100 Partisi)<br>
  ![2_2_100.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/2_2_100.jpg)<br>
  percobaan **berhasil** dieksekusi dengan waktu **24 detik**
- Percobaan 2 (2 Workers, 2 Cores, 1000 Partisi)<br>
  ![2_2_1000.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/2_2_1000.jpg)<br>
  percobaan **berhasil** dieksekusi dengan waktu **2.1 menit**
- Percobaan 3 (2 Workers, 4 Cores, 100 Partisi)<br>
  ![2_4_100.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/2_4_100.jpg)<br>
  percobaan **berhasil** dieksekusi dengan waktu **26 detik**
- Percobaan 4 (2 Workers, 4 Cores, 1000 Partisi)<br>
  ![2_4_1000.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/2_4_1000.jpg)<br>
  percobaan **berhasil** dieksekusi dengan waktu **2.1 menit**
- Percobaan 5 (5 Workers, 2 Cores, 100 Partisi)<br>
  ![5_2_100.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/5_2_100.jpg)<br>
  percobaan **berhasil** dieksekusi dengan waktu **53 detik**
- Percobaan 6 (5 Workers, 2 Cores, 1000 Partisi)<br>
  ![failed(3)_5_2_1000.jpg](<https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/failed(3)_5_2_1000.jpg>)<br>
  ![failed_5_2_1000.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/failed_5_2_1000.jpg)<br>
  ![failed(2)_5_2_1000.jpg](<https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/failed(2)_5_2_1000.jpg>)<br>
  percobaan **gagal** dieksekusi dan berhenti pada **1.7 menit**
- Percobaan 7 (5 Workers, 4 Cores, 100 Partisi)<br>
  ![failed(3)_5_4_100.jpg](<https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/failed(3)_5_4_100.jpg>)<br>
  ![failed(2)_5_4_100.jpg](<https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/failed(2)_5_4_100.jpg>)<br>
  ![failed_5_4_100.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/failed_5_4_100.jpg)<br>
  percobaan **gagal** dieksekusi dan berhenti pada **1.2 menit**
- Percobaan 8 (5 Workers, 4 Cores, 1000 Partisi)<br>
  ![failed(3)_5_4_1000.jpg](<https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/failed(3)_5_4_1000.jpg>)<br>
  ![failed_5_4_1000.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/failed_5_4_1000.jpg)<br>
  ![failed(4)_5_4_1000.jpg](<https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/failed(4)_5_4_1000.jpg>)<br>
  percobaan **gagal** dieksekusi dan berhenti pada **59 detik**

### **PERCOBAAN PENGARUH JUMLAH CORE TERHADAP SPEED**

- Percobaan 1 (1 Workers, 1 Cores, 1000 Partisi)<br>
  ![1_1_1000.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/1_1_1000.jpg)<br>
  percobaan **berhasil** dieksekusi dengan waktu **4 menit**
- Percobaan 2 (1 Workers, 2 Cores, 1000 Partisi)<br>
  ![1_2_1000.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/1_2_1000.jpg)<br>
  percobaan **berhasil** dieksekusi dengan waktu **2.2 menit**
- Percobaan 3 (1 Workers, 4 Cores, 1000 Partisi)<br>
  ![1_4_1000.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/1_4_1000.jpg)<br>
  percobaan **berhasil** dieksekusi dengan waktu **1.9 menit**
- Percobaan 4 (1 Workers, 8 Cores, 1000 Partisi)<br>
  ![1_8_1000.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/1_8_1000.jpg)<br>
  percobaan **berhasil** dieksekusi dengan waktu **2.0 menit**

### **PERCOBAAN PENGARUH JUMLAH WORKER TERHADAP SPEED**

- Percobaan 1 (1 Workers, 1 Cores, 1000 Partisi)<br>
  ![1_1_1000.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/1_1_1000.jpg)<br>
  percobaan **berhasil** dieksekusi dengan waktu **4 menit**
- Percobaan 2 (2 Workers, 1 Cores, 1000 Partisi)<br>
  ![2_1_1000.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/2_1_1000.jpg)<br>
  percobaan **berhasil** dieksekusi dengan waktu **2.4 menit**
- Percobaan 3 (3 Workers, 1 Cores, 1000 Partisi)<br>
  ![3_1_1000.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/3_1_1000.jpg)<br>
  percobaan **berhasil** dieksekusi dengan waktu **2.3 menit**
- Percobaan 4 (4 Workers, 1 Cores, 1000 Partisi)<br>
  ![4_1_1000.jpg](https://github.com/DJahvoe/Eksekusi-Script-Python-menggunakan-Apache-Spark-Cluster/blob/master/documentation/4_1_1000.jpg)<br>
  percobaan **berhasil** dieksekusi dengan waktu **2.6 menit**

### **TABEL HASIL PERCOBAAN**

| JUMLAH WORKER | JUMLAH CORE | JUMLAH PARTISI | STATUS EKSEKUSI | WAKTU EKSEKUSI |
| :-----------: | :---------: | :------------: | :-------------: | :------------: |
|               |             |                |                 |                |
|       2       |      2      |      100       |     SUCCESS     |    24 detik    |
|       2       |      2      |      1000      |     SUCCESS     |   2.1 menit    |
|       2       |      4      |      100       |     SUCCESS     |    26 detik    |
|       2       |      4      |      1000      |     SUCCESS     |   2.1 menit    |
|       5       |      2      |      100       |     SUCCESS     |    53 detik    |
|       5       |      2      |      1000      |     FAILED      |   1.7 menit    |
|       5       |      4      |      100       |     FAILED      |   1.2 menit    |
|       5       |      4      |      1000      |     FAILED      |    59 detik    |
|               |             |                |                 |                |
|       1       |      1      |      1000      |     SUCCESS     |    4 menit     |
|       1       |      2      |      1000      |     SUCCESS     |   2.2 menit    |
|       1       |      4      |      1000      |     SUCCESS     |   1.9 menit    |
|       1       |      8      |      1000      |     SUCCESS     |   2.0 menit    |
|       2       |      1      |      1000      |     SUCCESS     |   2.4 menit    |
|       3       |      1      |      1000      |     SUCCESS     |   2.3 menit    |
|       4       |      1      |      1000      |     SUCCESS     |   2.6 menit    |

### **KESIMPULAN DAN ASUMSI**

- Percobaan pada TASK

1. Dapat dipastikan bahwa apabila **jumlah partisi lebih besar** maka diperlukan waktu yang lebih lama.
2. Dari beberapa kasus percobaan yang dilakukan terdapat kasus dimana eksekusi tidak berhasil, diasumsikan bahwa eksekusi gagal dilakukan karena spesifikasi dari local-machine yang tidak memadai untuk memenuhi kebutuhan dari konfigurasi (overload).

- Percobaan pada Jumlah Core

1. **Jumlah core yang semakin banyak** membuat eksekusi **lebih cepat** dilakukan.
2. Terdapat kasus dimana **performa eksekusi** cenderung menurun saat jumlah core dinaikkan, diasumsikan bahwa local-machine melewati batas peak-performance.

- Percobaan pada Jumlah Worker

1. **Jumlah worker yang semakin banyak** membuat eksekusi **lebih cepat** dikarenakan pembagian tugas ke tiap-tiap slave.
2. Terdapat kasus dimana **performa eksekusi** cenderung menurun saat jumlah worker dinaikkan, diasumsikan bahwa local-machine melewati batas peak-performance (dan tidak disarankan mengeksekusi banyak worker dalam 1 local-machine yang sama karena membebani 1 local-machine).
