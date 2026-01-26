 Link : 
<link> https://drive.google.com/drive/folders/14zD5VKyev8gp7bceCZKrOBxnR7vXpIQt?usp=sharing </link> <br>

<p>
<b>Alur Kerja Aplikasi</b>

Aplikasi ini adalah aplikasi terdesentralisasi yang menghubungkan website dengan blockchain Sui. Setiap aksi user di website akan membuat transaksi ke blockchain, data akan disimpan di sana, dan kemudian ditampilkan kembali di website. Ini adalah proses siklis yang membuat aplikasi bisa berfungsi secara real-time.
1. User Membuka Aplikasi
Ketika user pertama kali membuka website di browser, mereka akan melihat interface React yang sudah siap untuk interaksi. Website menampilkan tombol untuk membuat greeting, menampilkan daftar greeting yang sudah ada, dan tombol untuk connect wallet.

2. User Menghubungkan Wallet
Sebelum melakukan apapun di blockchain, user harus terhubung dengan Sui Wallet extension. Wallet adalah identitas digital user - seperti username dan password yang dikombinasikan. Wallet memiliki private key (kunci pribadi) yang digunakan untuk menandatangani setiap transaksi. Tanpa wallet, user tidak bisa melakukan transaksi karena blockchain membutuhkan bukti bahwa transaksi benar-benar dari user yang sah.

3. User Membuat Greeting
Ketika user klik tombol "Create Greeting", frontend (React) mempersiapkan sebuah transaksi yang berisi instruksi untuk menjalankan fungsi new() dari smart contract. Frontend tahu smart contract mana yang harus dipanggil karena sudah tersimpan PackageID di file constants.ts.

4. Wallet Menandatangani Transaksi
Sebelum transaksi dikirim ke blockchain, wallet extension akan muncul dan meminta user untuk approve/confirm. Wallet kemudian menandatangani transaksi dengan private key user. Tanda tangan ini membuktikan bahwa transaksi benar-benar dari user yang sah, dan tidak ada yang bisa memalsukan.

5. Blockchain Menerima & Mengeksekusi
Transaksi yang sudah ditandatangani dikirim ke blockchain Sui. Node-node di blockchain akan memvalidasi signature dan mengeksekusi fungsi new() dari smart contract. Smart contract kemudian membuat Greeting object baru dengan text "Hello world!" dan menyimpannya sebagai shared object.

6. Blockchain Menyimpan Data
Greeting object yang baru dibuat disimpan secara permanen di blockchain dengan object ID unik. Karena ini adalah shared object, siapa saja di network bisa mengaksesnya dan mengedit textnya menggunakan fungsi update_text(). Data tidak bisa dihapus atau dimanipulasi karena blockchain bersifat immutable (tidak bisa diubah).

7. Website Menampilkan Data
Frontend menerima response dari blockchain yang berisi object ID dan informasi greeting yang baru dibuat. Data ini kemudian ditampilkan di UI sehingga user bisa melihat greeting yang baru saja mereka buat. User lain juga bisa melihat greeting ini karena disimpan di blockchain yang public.

8. Siapa Saja Bisa Edit
Salah satu fitur unik dari aplikasi ini adalah siapa saja bisa mengedit teks greeting yang sudah ada. Ini dimungkinkan karena greeting adalah shared object. Ketika user lain ingin mengedit greeting, mereka akan menjalankan fungsi update_text() dengan object ID greeting tersebut, dan perubahan akan disimpan di blockchain.
</p>
