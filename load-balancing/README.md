<h1 align "center"> Alur Aplikasi </h1>

<b>1.</b> Client request ke http://localhost
Browser user mengirim permintaan ke server di port 80 <br>
<b>2.</b> Nginx (port 80) terima request
Nginx adalah load balancer yang mendengarkan semua request masuk <br>
<b>3.</b> Nginx forward ke App Instance 1 atau 2 (bergantian)
Nginx membagi beban: request 1 ke App 1, request 2 ke App 2, dst (round-robin)
Ini mencegah satu app kewalahan<br>
<b>4.</b> Controller (home.py) handle request
Request URL / diarahkan ke method index(), /example ke method example()
Controller adalah "dispatcher" yang menentukan aksi apa yang harus dijalankan<br>
<b>5.</b> Service (services.py) proses logic
Menyiapkan data dan business logic yang diperlukan untuk response<br>
<b>6.</b> Template (views/home/*.jinja) render HTML
Template HTML diisi dengan data dari service
Jinja2 engine mengkonversi template menjadi HTML final<br>
<b>7.</b> Response kembali ke client
HTML yang sudah dirender dikirim balik ke browser client untuk ditampilkan<br>


