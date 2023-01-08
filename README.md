# SIG_TEORI_TGS_21
 Creating Heatmaps
Langkah Kerja Peta 7: Creating Heatmaps (QGIS3)

1. Pertama-tama kita akan memuat layer peta dasar dari OpenStreetMap dan kemudian mengimpor data CSV. Di tab Browser, gulir ke bawah dan temukan bagian Ubin XYZ.

2. Bentangkan untuk melihat layer petak OpenStreetMap. Seret dan lepas ke kanvas utama. Selanjutnya kita akan memuat file CSV. Klik tombol Buka Pengelola Sumber Data.

3. Beralih ke tab Teks Dibatasi. Di sini kita akan mengimpor data kejahatan yang datang dalam file teks format CSV. Klik tombol … di sebelah Nama file dan telusuri file 2019-02-surrey-street.csv yang diunduh. Bidang X dan bidang Y di bagian Definisi Geometri akan diisi secara otomatis dengan kolom Bujur dan Lintang. CRS Geometri harus dibiarkan ke standar EPSG:4326 - definisi WGS 84. Pastikan data terlihat benar di panel Data sampel dan klik Tambah, diikuti dengan Tutup.

4. Anda akan melihat 2 lapisan - OpenStreetMap dan 2019-02-surrey-street dimuat di panel Lapisan QGIS. Klik kanan layer 2019-02-surrey-street dan pilih Zoom to Layer.

5. Anda akan melihat layer poin insiden kejahatan dihamparkan pada peta dasar OpenStreetMap. Zoom dan Pan untuk menjelajahi data. Datanya cukup padat dan sulit untuk mengetahui di mana terdapat konsentrasi kejahatan yang tinggi. Di sinilah visualisasi peta panas akan berguna. Pilih layer 2019-02-surrey-street dan klik tombol panel Open the Layer Styling.

6. Pilih Heatmap sebagai perender di menu dropbox. Panel Layer Styling bersifat interaktif dan Anda dapat segera melihat efek perubahan Anda tercermin di kanvas. Lapisan sekarang akan ditampilkan di jalan warna skala abu-abu default.

7. Peta panas biasanya dirender menggunakan jalur warna kuning-ke-merah atau putih-ke-merah di mana konsentrasi titik yang lebih tinggi menghasilkan lebih banyak panas. Klik menu dropdown Color ramp dan pilih Reds color-ramp.

8. Selanjutnya Anda harus memilih Radius. Parameter ini menentukan lingkungan melingkar di sekitar setiap titik di mana titik tersebut akan memiliki pengaruh. Nilai ini akan sangat bergantung pada jenis data masukan Anda. Untuk data kami, anggap saja insiden kejahatan akan berdampak hingga 5 Kilometer dari lokasi. Perhatikan bahwa CRS proyek saat ini diatur ke EPSG: 3857 di sudut kanan bawah. CRS ini memiliki satuan meter, jadi kita harus menentukan 5000 meter sebagai radiusnya. Parameter lain yang disembunyikan dari menu ini adalah bentuk Kernel. Ini adalah fungsi yang menentukan bagaimana pengaruh suatu titik harus tersebar pada radius yang diberikan. Perender Heatmap menggunakan fungsi Quartic untuk perhitungan ini. Ada jenis kernel lain seperti Triangular, Uniform, Triweight, dan Epanechnikov yang dapat ditentukan saat menggunakan metode pembuatan peta panas berbeda yang dijelaskan nanti di tutorial ini. Lihat posting ini untuk penjelasan dan panduan yang bagus untuk memilih radius dan bentuk kernel yang tepat.

9. Visualisasi peta panas sudah siap. Kita bisa mengatur Opacity dari heatmap di bagian Layer Rendering di bagian bawah. Atur opacity menjadi 60% agar Anda dapat melihat peta dasar beserta peta panasnya.

10. Untuk banyak jenis analisis, hanya mempertimbangkan kerapatan poin sudah cukup baik. Namun terkadang, Anda mungkin ingin memberikan kepentingan yang berbeda untuk setiap poin. Kejahatan yang lebih kejam seharusnya memiliki pengaruh lebih besar pada peta panas keluaran daripada perampokan. Demikian pula, kadang-kadang suatu titik dapat mewakili beberapa pengamatan di satu lokasi yang perlu diperhitungkan dalam analisis. Untuk melakukannya, Anda dapat menyediakan kolom bobot numerik opsional yang menentukan nilai untuk setiap titik. Mari tambahkan bidang bobot dan gunakan untuk menyempurnakan peta panas. Klik kanan layer 2019-02-surrey-street dan pilih Open Attribute Table.

11. Anda akan melihat bidang teks bernama Jenis kejahatan di data input yang menjelaskan jenis kejahatan. Kita dapat menggunakan ini untuk mengkategorikan berbagai jenis kejahatan dan menetapkan bobot yang lebih tinggi untuk kejahatan yang lebih kejam.

12. Klik Kalkulator lapangan terbuka.

13. Kami sekarang akan memasukkan rumus yang menggunakan jenis Kejahatan dan menentukan nilai bobotnya. QGIS memiliki cara praktis untuk menambahkan bidang yang dihitung menggunakan Bidang Virtual. Bidang virtual disimpan dalam proyek QGIS dan tidak mengubah data sumber. Ini juga dihitung secara dinamis dan dapat digunakan di mana saja di QGIS seperti halnya nilai atribut lainnya. Masukkan bobot sebagai nama bidang Keluaran dan atur jenis bidang Keluaran ke Bilangan bulat (bilangan bulat). Masukkan ekspresi berikut di Editor ekspresi. Di sini kita menggunakan pernyataan CASE untuk menetapkan nilai yang berbeda berdasarkan kondisi yang berbeda. Klik Oke.

CASE WHEN "Crime type" LIKE 'Violence%' THEN 10 WHEN "Crime type" LIKE 'Criminal%' THEN 5 ELSE 1 END

14. Atribut baru akan ditambahkan untuk setiap fitur dengan nilai bobot yang sesuai.

15. Kembali ke panel Layer Styling, klik menu drop-down untuk Weight points by dan pilih bidang bobot yang baru ditambahkan.

16. Anda akan melihat perubahan rendering peta panas untuk memperhitungkan parameter bobot. Tutup panel Layer Styling.

17. Jika Anda memerlukan visualisasi peta panas untuk disimpan sebagai lapisan raster permanen atau ingin menyesuaikan peta panas dengan opsi lanjutan seperti kernel yang berbeda atau radius dinamis, Anda dapat menggunakan Peta Panas (Estimasi Kepadatan Kernel) dari Kotak Alat Pemrosesan. Kami sekarang akan menggunakan algoritma ini. Pergi ke Memproses ‣ Toolbox.

18. Sebelum kita dapat membuat peta panas, kita perlu memproyeksikan ulang data sumber ke CRS yang diproyeksikan. Karena jarak memainkan peran penting dalam perhitungan peta panas, tidak benar menggunakan CRS geografis. Cari dan temukan algoritma Vector general ‣ Proyeksi ulang layer.

19. Pada dialog Reproject layer, klik tombol Select CRS untuk Target CRS. Cari dan pilih EPSG:27700 OSGB 1936 / British National Grid CRS. CRS yang diproyeksikan ini adalah pilihan yang baik untuk data di Inggris. Klik Jalankan.

20. Sebuah layer baru bernama Reprojected akan ditambahkan ke panel Layers. Hapus centang pada kotak di sebelah layer lama 2019-02-surrey-street untuk menyembunyikannya.

21. Cari dan temukan algoritma Interpolation ‣ Heatmap (Kernel Density Estimation).

22. Pada dialog Heatmap (Kernel Density Estimation), kita akan menggunakan parameter yang sama seperti sebelumnya. Pilih Radius sebagai 5000 meter dan Weight from field sebagai weight. Atur ukuran Pixel X dan ukuran Pixel Y menjadi 50 meter. Biarkan Kernel membentuk nilai default Quartic. Klik Jalankan.

Catatan : Parameter Radius from field memungkinkan Anda menentukan radius pencarian dinamis untuk setiap titik. Ini dapat digunakan bersama dengan Bobot dari bidang untuk memiliki kontrol yang lebih baik tentang bagaimana pengaruh setiap titik tersebar.

23. Setelah pemrosesan selesai, lapisan raster baru bernama OUTPUT akan dimuat. Visualisasi default jelek karena menggunakan penyaji abu-abu Singleband. Klik tombol panel Open the Layer Styling.

24. Ubah render menjadi Singleband Pseudocolor dan pilih jalur warna Merah. Lapisan sekarang terlihat seperti visualisasi peta panas yang telah kita buat sebelumnya.
