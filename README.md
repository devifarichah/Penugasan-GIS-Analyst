**Peta Proyeksi Tutupan Lahan Kawasan Aglomerasi Sarbagita**

Proyek ini disusun sebagai bagian dari penugasan rekrutmen Tenaga Ahli _GIS Analyst,_ untuk menghasilkan peta proyeksi perubahan tutupan lahan _(land cover)_ pada Kawasan Aglomerasi Sarbagita (Denpasar, Badung, Gianyar, Tabanan), Provinsi Bali, sebagai dukungan analisis spasial terhadap tata kelola pemanfaatan ruang.

**1. Deskripsi & Tujuan Proyek**
Analisis ini bertujuan untuk:
a. Mengklasifikasikan tutupan lahan Kawasan Sarbagita pada tahun 2020 dan 2023
b. Memproyeksikan kondisi tutupan lahan pada tahun 2026 berdasarkan pola perubahan yang terjadi antara 2020-2023

**2. Data yang Digunakan**
a. Citra Sentinel 2A Composite	(https://bit.ly/Sentinel2A_Sarbagita)
b. Sistem koordinat awal	EPSG:4326 (WGS 84)	di_reproject_ ke EPSG:32750 (UTM Zone 50S)
c. Training sample	melalui pengambilan titi di QGIS sebanyak 15 pada setiap kelas

3. Kelas Tutupan Lahan
Klasifikasi menggunakan 3 kelas tutupan lahan, dengan mempertimbangkan keterbatasan waktu pengerjaan:
Kode Kelas	1 untuk vegetasi atau hutan
Kode Kelas	2	untuk lahan terbangun
Kode Kelas	3	untuk pertanian atau sawah

4. Metodologi
Alur kerja analisis dilakukan sepenuhnya menggunakan bahasa pemrograman Python di lingkungan Google Colab, dengan tahapan sebagai berikut:
a. Praproses citra
Citra Sentinel 2A composite (2020 & 2023) di_reproject_ dari sistem koordinat geografis (EPSG:4326) ke sistem proyeksi UTM Zone 50S (EPSG:32750), untuk memastikan satuan spasial dalam meter dan konsistensi ukuran piksel.
b. Pembuatan _training sample_
_Training sample_ dibuat secara manual melalui digitasi titik _(point)_ di QGIS, dengan interpretasi visual pada citra _composite._ Setiap titik diberi atribut class_id dan class_name sesuai kelas tutupan lahan pada lokasi tersebut. _Training sample_ dibuat sama untuk tahun 2020 dan 2023 karena setelah dilakukan pengecekan pada titik-titik tersebut tidak ada perubahan tutupan lahan antar periode.
c. Klasifikasi tutupan lahan
Nilai piksel pada lokasi titik training sample diekstraksi sebagai data _training._
Model klasifikasi Random Forest dilatih secara terpisah untuk citra tahun 2020 dan 2023.
Model diterapkan ke seluruh piksel citra untuk menghasilkan peta klasifikasi tutupan lahan tahun 2020 dan 2023.
d. Uji Akurasi
Validitas model diuji menggunakan data uji dengan menghitung:
Overall Accuracy (OA)
Cohen's Kappa Index
Producer's Accuracy (PA) per kelas
User's Accuracy (UA) per kelas
e. Proyeksi tutupan lahan 2026
Matriks transisi Markov dihitung dari perbandingan piksel peta klasifikasi 2020 dan 2023, menghasilkan probabilitas perubahan antar kelas.
Probabilitas transisi tersebut diasumsikan berulang pada periode berikutnya (2023 → 2026, rentang 3 tahun yang sama), dan diterapkan secara probabilistik pada peta 2023 untuk menghasilkan peta proyeksi 2026.

5. Hasil uji akurasi
a. Hasil klasifiasi tahun 2020
Overall Accuracy: 100% Cohen's Kappa: 1.0
a. Hasil klasifiasi tahun 2023
Overall Accuracy: 100% Cohen's Kappa: 1.0

6. Limitasi Analisis
a. Jumlah kelas tutupan lahan disederhanakan menjadi 3 kelas (vegetasi, lahan terbangun, pertanian), belum memisahkan sub-kelas yang lebih detail (misalnya badan air atau lahan terbuka sebagai kelas tersendiri).
b. Jumlah titik training sample per kelas (15), yang berada pada batas minimum.
c. Terdapat area nodata di tepi citra hasil reprojection yang disembunyikan secara visual pada tahap penayangan peta, namun tidak memengaruhi hasil perhitungan uji akurasi karena training sample diambil eksklusif dari piksel bertutupan lahan valid.

7. Cara menjalankan ode
a. Buka notebook devi_farichah.py di Google Colab
b. Pastikan seluruh file data (citra .tif dan training sample .shp beserta file pendukungnya) sudah diunggah ke folder Google Drive yang sesuai dengan path yang tertulis di notebook
c. Jalankan seluruh cell secara berurutan dari atas ke bawah (Runtime → Run all)
d. Seluruh output (peta klasifikasi, peta proyeksi, tabel akurasi, dan visualisasi) akan otomatis tersimpan di folder kerja yang telah ditentukan
