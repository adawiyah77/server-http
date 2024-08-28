# New API

>[!CATATAN]
> Proyek ini adalah proyek sumber terbuka, dengan pengembangan sekunder berdasarkan [Satu API](https://github.com/songquanpeng/one-api)

> [!PENTING]
> Pengguna harus menggunakannya sesuai dengan [Ketentuan Penggunaan](https://openai.com/policies/terms-of-use) OpenAI dan **hukum dan peraturan**, dan tidak boleh menggunakannya untuk tujuan ilegal.
> Proyek ini hanya untuk studi pribadi, stabilitas tidak dijamin, dan tidak ada dukungan teknis yang diberikan.
> Sesuai dengan persyaratan [Tindakan Sementara untuk Pengelolaan Layanan Kecerdasan Buatan Generatif] (http://www.cac.gov.cn/2023-07/13/c_1690898327029107.htm), harap jangan memberikan informasi apa pun yang tidak sah kepada publik di Tiongkok.

> [!TIPS]
> Gambar Docker versi terbaru: `kalsium/api-baru:terbaru`
>Kata sandi root akun default 123456
> Perbarui perintah:
> ```
> docker run --rm -v /var/run/docker.sock:/var/run/docker.sock mengandungrrr/watchtower -cR
> ```


## Perubahan besar
Perubahan utama pada versi fork ini adalah sebagai berikut:

1. Antarmuka UI baru (beberapa antarmuka masih perlu diperbarui)
2. Tambahkan dukungan untuk antarmuka [Midjourney-Proxy(Plus)](https://github.com/novicezk/midjourney-proxy), [Dokumen Docking](Midjourney.md)
3. Mendukung fungsi isi ulang online, yang dapat diatur dalam pengaturan sistem. Antarmuka pembayaran yang saat ini didukung:
    + [x] Pembayaran Mudah
4. Dukungan menggunakan kunci untuk menanyakan kuota penggunaan:
+ Bekerja sama dengan proyek [neko-api-key-tool](https://github.com/Calcium-Ion/neko-api-key-tool) untuk mewujudkan kueri menggunakan kunci
5. Saluran menampilkan kuota yang terpakai dan mendukung akses oleh organisasi yang ditunjuk.
6. Pagination mendukung pemilihan jumlah tampilan per halaman
7. Kompatibel dengan database One API asli, Anda dapat langsung menggunakan database asli (one-api.db)
8. Mendukung pengisian model bayar per penggunaan, yang dapat diatur di Pengaturan Sistem-Pengaturan Operasi
9. Saluran dukungan **berbobot acak**
10. Dasbor data
11. Model yang dapat dipanggil dengan token dapat diatur
12. Mendukung login resmi Telegram.
    1. Pengaturan Sistem - Konfigurasikan Registrasi Login - Izinkan Login melalui Telegram
2. Masukkan perintah /setdomain untuk [@Botfather](https://t.me/botfather)
    3. Pilih bot Anda dan masukkan http(s)://alamat/login situs web Anda
    4. Nama Bot Telegram adalah rangkaian nama pengguna bot dikurangi @
13. Tambahkan dukungan untuk antarmuka [Suno API](https://github.com/Suno-API/Suno-API), [Dokumen Docking](Suno.md)
14. Mendukung model Rerank, saat ini hanya kompatibel dengan Cohere dan Jina, dapat dihubungkan ke Dify, [dokumen docking] (Rerank.md)

## Dukungan model
Versi ini juga mendukung model berikut:
1. Model pihak ketiga **gps** (gpt-4-gizmo-*)
2. Spektrum cerdas glm-4v, pengenalan gambar glm-4v
3. Claude Antropis 3
4. [Ollama](https://github.com/ollama/ollama?tab=readme-ov-file), saat menambahkan saluran, kuncinya dapat diisi dengan santai. localhost:11434 ](http://localhost:11434), jika perlu dimodifikasi, silakan modifikasi di saluran
5. Antarmuka [Midjourney-Proxy(Plus)](https://github.com/novicezk/midjourney-proxy), [Dokumen docking](Midjourney.md)
6. [ Nol Seribu Hal ](https://platform.lingyiwanwu.com/)
7. Sesuaikan saluran dan dukung pengisian alamat panggilan lengkap
8. Antarmuka [Suno API](https://github.com/Suno-API/Suno-API), [Dokumen docking](Suno.md)
9. Model rerank saat ini mendukung [Cohere](https://cohere.ai/) dan [Jina](https://jina.ai/), [Dokumen Docking](Rerank.md)
10.Difikasi

Anda dapat menambahkan model khusus gpt-4-gizmo-* ke saluran. Model ini bukan model OpenAI resmi, tetapi model pihak ketiga dan tidak dapat dipanggil menggunakan kunci resmi.

## Lebih banyak konfigurasi daripada One API asli
- `STREAMING_TIMEOUT`: Menyetel batas waktu untuk streaming balasan, defaultnya adalah 30 detik.
- `DIFY_DEBUG`: Mengatur apakah saluran Dify mengeluarkan alur kerja dan informasi node ke klien, defaultnya adalah `true`.
- `FORCE_STREAM_OPTION`: Apakah akan mengganti parameter stream_options klien dan meminta upstream untuk mengembalikan penggunaan mode streaming. Defaultnya adalah `true`. Direkomendasikan untuk mengaktifkannya. Ini tidak akan mempengaruhi hasil yang dikembalikan oleh parameter stream_options klien .
- `GET_MEDIA_TOKEN`: Ini adalah token gambar statistik. Defaultnya adalah `true`. Setelah dimatikan, token gambar tidak lagi dihitung secara lokal, yang mungkin mengakibatkan penagihan berbeda dari upstream opsi GET_MEDIA_TOKEN_NOT_STREAM`.
- `GET_MEDIA_TOKEN_NOT_STREAM`: Apakah akan menghitung token gambar dalam situasi non-streaming (`stream=false`), defaultnya adalah `true`.
- `UPDATE_TASK`: Apakah akan memperbarui tugas asinkron (Midjourney, Suno), defaultnya adalah `true`, kemajuan tugas tidak akan diperbarui setelah penutupan.

## Penerapan
### Persyaratan penerapan
- Basis data lokal (default): SQLite (Penyebaran Docker menggunakan SQLite secara default, dan direktori `/data` harus dipasang ke host)
- Basis data jarak jauh: versi MySQL >= 5.7.8, versi PgSQL >= 9.6
### Penerapan berdasarkan Docker
``` cangkang
# Gunakan perintah penerapan SQLite:
docker run --name new-api -d --restart selalu -p 3000:3000 -e TZ=Asia/Shanghai -v /home/ubuntu/data/new-api:/data kalsiumion/new-api:latest
# Gunakan perintah penerapan MySQL dan tambahkan `-e SQL_DSN="root:123456@tcp(localhost:3306)/oneapi"` berdasarkan hal di atas. Silakan ubah sendiri parameter koneksi database.
# Misalnya:
docker run --name new-api -d --restart selalu -p 3000:3000 -e SQL_DSN="root:123456@tcp(localhost:3306)/oneapi" -e TZ=Asia/Shanghai -v /home/ubuntu /data/new-api:/data kalsiumion/new-api:latest
```
### Deploy menggunakan fungsi Docker pada panel Pagoda
``` cangkang
# Gunakan perintah penerapan SQLite:
docker run --name new-api -d --restart selalu -p 3000:3000 -e TZ=Asia/Shanghai -v /www/wwwroot/new-api:/data kalsiumion/new-api:latest
# Gunakan perintah penerapan MySQL dan tambahkan `-e SQL_DSN="root:123456@tcp(localhost:3306)/oneapi"` berdasarkan hal di atas. Silakan ubah sendiri parameter koneksi database.
# Misalnya:
# Catatan: Akses jarak jauh harus diaktifkan untuk database, dan hanya akses IP server yang diperbolehkan.
docker run --name new-api -d --restart selalu -p 3000:3000 -e SQL_DSN="root:123456@tcp(alamat server Pagoda: port basis data Pagoda)/nama basis data Pagoda" -e TZ=Asia/ Shanghai -v /www/wwwroot/new-api:/data kalsiumion/new-api:latest
# Catatan: Basis data harus mengaktifkan akses jarak jauh dan hanya mengizinkan akses IP server.
```

## Coba ulang saluran
Fungsi percobaan ulang saluran telah diterapkan. Anda dapat mengatur jumlah percobaan ulang di Pengaturan->Pengaturan Operasi->Pengaturan Umum.  
Jika fungsi coba ulang diaktifkan, percobaan ulang pertama akan menggunakan prioritas yang sama, percobaan ulang kedua akan menggunakan prioritas berikutnya, dan seterusnya.
### Metode pengaturan cache
1. `REDIS_CONN_STRING`: Setelah pengaturan, Redis akan digunakan sebagai cache.
    + Contoh: `REDIS_CONN_STRING=redis://default:redispw@localhost:49153`
2. `MEMORY_CACHE_ENABLED`: Aktifkan cache memori (jika `REDIS_CONN_STRING` disetel, tidak diperlukan pengaturan manual), yang akan menyebabkan penundaan tertentu dalam memperbarui kuota pengguna. Nilai opsional adalah `benar` dan `salah` Jika tidak disetel, defaultnya adalah `false`.
+ Contoh: `MEMORY_CACHE_ENABLED=benar`
### Mengapa terkadang tidak ada percobaan ulang?
Kode kesalahan ini tidak akan dicoba lagi: 400, 504, 524
### Saya ingin 400 dicoba ulang juga
Di `Saluran->Edit`, ubah `Salinan Kode Status` menjadi
``` json
{
  "400": "500"
}
```
Anda dapat mengubah kesalahan 400 menjadi kesalahan 500 sehingga Anda dapat mencoba lagi

## Dokumentasi pengaturan antarmuka tengah perjalanan
[Dokumen dok](Midjourney.md)

## Dokumentasi pengaturan antarmuka Suno
[Dokumen dok](Suno.md)

## Grup komunikasi
<img src="https://github.com/Calcium-Ion/new-api/assets/61247483/de536a8a-0161-47a7-a0a2-66ef6de81266" width="300">

## Tangkapan layar antarmuka
![gambar](https://github.com/Calcium-Ion/new-api/assets/61247483/ad0e7aae-0203-471c-9716-2d83768927d4)

![gambar](https://github.com/Calcium-Ion/new-api/assets/61247483/d1ac216e-0804-4105-9fdc-66b35022d861)

![gambar](https://github.com/Calcium-Ion/new-api/assets/61247483/3ca0b282-00ff-4c96-bf9d-e29ef615c605)
![gambar](https://github.com/Calcium-Ion/new-api/assets/61247483/f4f40ed4-8ccb-43d7-a580-90677827646d)
![gambar](https://github.com/Calcium-Ion/new-api/assets/61247483/90d7d763-6a77-4b36-9f76-2bb30f18583d)
![gambar](https://github.com/Calcium-Ion/new-api/assets/61247483/e414228a-3c35-429a-b298-6451d76d9032)
Modus malam
![gambar](https://github.com/Calcium-Ion/new-api/assets/61247483/1c66b593-bb9e-4757-9720-ff2759539242)

![gambar](https://github.com/Calcium-Ion/new-api/assets/61247483/5b3228e8-2556-44f7-97d6-4f8d8ee6effa)
![gambar](https://github.com/Calcium-Ion/new-api/assets/61247483/af9a07ee-5101-4b3d-8bd9-ae21a4fd7e9e)

## Proyek terkait
- [Satu API](https://github.com/songquanpeng/one-api): proyek asli
- [Midjourney-Proxy](https://github.com/novicezk/midjourney-proxy): Dukungan antarmuka tengah perjalanan
- [chatnio](https://github.com/Deeptrain-Community/chatnio): Solusi B/C terpadu AI generasi berikutnya
- [neko-api-key-tool](https://github.com/Calcium-Ion/neko-api-key-tool): Gunakan kunci untuk menanyakan kuota penggunaan



## Sejarah Bintang

[![Bagan Sejarah bintang](https://api.star-history.com/svg?repos=Calcium-Ion/new-api&type=Date)](https://star-history.com/#Calcium-Ion/new- api&Tanggal)
