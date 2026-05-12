a. How much data your publisher program will send to the message broker in one run?
Dalam satu kali eksekusi program (satu kali jalan), program publisher ini akan mengirimkan 5 buah pesan (event) ke message broker. Kelima pesan tersebut dikirimkan secara berurutan ke dalam queue bernama "user_created", di mana masing-masing pesan membawa data objek UserCreatedEventMessage yang berisi ID dan nama pengguna (untuk Amir, Budi, Cica, Dira, dan Emir).

b. The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber program, what does it mean?
Kesamaan URL ini berarti program publisher dan subscriber terhubung ke server message broker (RabbitMQ) yang sama persis, yaitu yang sedang berjalan di komputer lokal kita (melalui Docker) pada port 5672.

Hal ini memang wajib dilakukan dalam arsitektur event-driven. Agar subscriber bisa menerima dan memproses pesan yang dikirimkan oleh publisher, keduanya harus nongkrong dan berkomunikasi melalui jalur perantara (broker) yang sama. Jika URL nya berbeda, pesan yang dikirim publisher tidak akan pernah sampai ke subscriber.



### Running RabbitMQ
![RabbitMQ Dashboard](images/rabbitmq_running.png)

### Sending and Processing Event
![Sending and Processing Event](images/sending_processing.png)


**What happens here:**
Ketika perintah `cargo run` dieksekusi pada direktori *publisher*, program tersebut mempublikasikan 5 buah *event* berupa pesan `UserCreatedEventMessage` ke *message broker* (RabbitMQ). Karena program *subscriber* sudah dalam posisi berjalan (*standby*) dan terhubung ke *broker* yang sama, *subscriber* tersebut secara otomatis langsung menerima (*consume*) dan memproses kelima pesan yang masuk, lalu mencetak isinya secara *real-time* ke layar console.


### Monitoring Chart Based on Publisher
![Message Rates Chart](images/message_rates_chart.png)

**Penjelasan:**
Lonjakan (*spike*) yang terjadi pada grafik "Message rates" di dashboard RabbitMQ berbanding lurus dengan eksekusi program *publisher*. Setiap kali kita menjalankan program *publisher* (`cargo run`), program tersebut mempublikasikan 5 pesan sekaligus ke *message broker* dalam waktu yang sangat singkat. Karena RabbitMQ langsung menerima pesan (*publish*) dan meneruskannya ke *subscriber* (*deliver*) pada detik yang sama, aktivitas ini terekam sebagai lonjakan tajam pada grafik. Saat program selesai mengirim, grafik akan kembali turun ke titik nol (*idle*).

