# Tugas Subnetting
## Komunikasi Data dan Jaringan Komputer


Nama: Jofanka Al-Kautsar Pangestu Abady

NRP: 5027241107

Mata Kuliah: Komunikasi Data dan Jaringan Komputer (C)

## Topologi
Untuk Topologi sebagai berikut.

<img width="1376" height="620" alt="gambar" src="https://github.com/user-attachments/assets/f08029fe-b544-44d0-8acd-26d4d84f0332" />







## Subnetting
Subnetting adalah proses memecah jaringan menjadi bagian-bagian kecil. Tujuannya adalah untuk mempermudah konfigurasi jaringan. 
### A. VLSM
#### 1) Pendahuluan

Laporan ini merinci proses alokasi alamat IP untuk jaringan Yayasan Pendidikan ARA. Tujuan utamanya adalah merancang skema pengalamatan jaringan yang efisien menggunakan Variable Length Subnet Mask (VLSM) untuk memenuhi kebutuhan host di Kantor Pusat dan Kantor Cabang, serta menghitung CIDR (Supernetting) untuk agregasi rute.

#### Parameter Alokasi IP

Perhitungan didasarkan pada parameter unik yang diberikan dalam soal:

| Parameter | Value |
| --- | --- |
| NRP | 5027241107 |
| Pembagi | 256 |
| NRP mod 256 | 147 |
| Base | 10.147.0.0 |

#### 2) Metodologi VLSM

Perhitungan VLSM dilakukan berdasarkan best practice dengan mengurutkan semua kebutuhan jaringan (total 8 subnet) dari jumlah host terbesar ke terkecil. Alokasi IP address dilakukan secara berurutan (sequential) untuk memastikan tidak ada IP yang tumpang tindih dan memaksimalkan efisiensi alokasi.

Kebutuhan 8 subnet tersebut adalah:

1. Sekretariat: 380 host
2. Bidang Kurikulum: 220 host
3. Bidang Guru & Tendik: 95 host
4. Bidang Sarana Prasarana: 45 host
5. Bidang Pengawas Sekolah (Branch): 18 host
6. Bidang Pengawas Sekolah (Cabang): 18 host
7. Server & Admin: 6 host
8. Link WAN (Antar-Router): 2 host

#### 3) Tabel Hasil Perhitungan VLSM

Berikut adalah tabel alokasi final yang mencakup semua 8 subnet, sesuai dengan yang tertera pada spreadsheet.


| Ruang | Jumlah Host Aktif | Prefix | Column 1 | Oktet Terakhir | Oktet Terakhir (binary) | NID | Broadcast | IP Available | Available Hosts | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Sekretariat | 380 | /23 | 512 | 512 | 1000000000 | 10.147.0.0 | 10.147.1.255 | 10.147.0.1 - 10.147.2.255 | 510 | 
| Bidang Kurikulum | 220 | /24 | 256 | 768 | 1100000000 | 10.147.2.0 | 10.147.2.255 | 10.147.1.1 - 10.147.1.126 | 254 | 
| Bidang Guru & Tendik | 95 | /25 | 128 | 896 | 1110000000 | 10.147.3.0 | 10.147.3.127 | 10.147.3.1 - 10.147.3.126 | 126 | 
| Bidang Sarana Prasarana | 45 | /26 | 64 | 960 | 1111000000 | 10.147.3.128 | 10.147.3.191 | 10.147.3.129 - 10.147.3.190 | 62 | 
| Bidang Pengawas Sekolah (Branch) | 18 | /27 | 32 | 992 | 1111100000 | 10.147.3.192 | 10.147.3.223 | 10.147.3.193 - 10.147.3.222 | 30 | 
| Bidang Pengawas Sekolah | 18 | /27 | 32 | 1024 | 10000000000 | 10.147.3.224 | 10.147.3.255 | 10.147.3.225 - 10.147.3.254 | 30 | 
| Server & Admin | 6 | /29 | 8 | 1032 | 10000001000 | 10.147.4.0 | 10.147.4.7 | 10.147.1.159 - 10.147.1.162 | 6 | 
| Link WAN | 2 | /30 | 4 | 1036 | 10000001100 | 10.147.4.8 | 10.147.4.11 | 10.147.4.9 - 10.147.4.10 | 2 | 

Untuk lebih jelasnya dapat dilihat pada link berikut.

[Tabel VLSM](https://docs.google.com/spreadsheets/d/1K39cFSm9jZnCzlMsCC4zXNYzNXlhpiq7Xf0Oa_5CWek/edit?gid=0#gid=0)


#### 4) Mengapa Menggunakan Prefix /21?
Jika kita lihat dari tabel alokasinya, IP yang dihasilkan mencapai `10.147.4.11`, yang di mana penggunaan prefix `/22` tidak cukup untuk jaringan tersebut.


### CIDR

#### 1) Definisi dan Tujuan
CIDR (Classless Inter-Domain Routing) adalah metode pengalamatan dan subnetting jaringan IP yang tidak terikat pada batasan kelas tradisional (Kelas A, B, C). Tujuan dari CIDR adalah untuk meringkas semua 8 subnet VLSM yang telah dialokasikan untuk Yayasan Pendidikan ARA menjadi satu alamat jaringan tunggal (Supernet).

#### 2) Data Input

Kita akan menghitung IP berdasarkan permintaan host. Penggunaan label diperlukan untuk menandai sebuah subnet.

| Ruang | Jumlah Host Aktif | Prefix | Column1 | Subnet |
| --- | --- | --- | --- | --- |
| Sekretariat | 380 | /23 | 512 | A6 |
| Bidang Kurikulum | 220 | /24 | 256 | A2 |
| Bidang Guru & Tendik | 95 | /25 | 128 | A1 |
| Bidang Sarana Prasarana | 45 | /26 | 64 | A3 |
| Bidang Pengawas Sekolah (Branch) | 18 | /27 | 32 | A4 |
| Bidang Pengawas Sekolah | 18 | /27 | 32 | A8 |
| Server & Admin | 6 | /29 | 8 | A5 |
| Router WAN | 2 | /30 | 4 | A7 |

#### 3) Supernetting
Setelah mendapatkan data input, langkah selanjutnya kita akan melakukan perhitungan untuk mencari IP awal dan IP akhir. Untuk tabelnya sebagai berikut.

| Subnet | NID | Broadcast|
| --- | --- | --- |
| C1 | 10.147.0.0 | 10.147.7.255 |
| B1 | 10.147.0.0 | 10.147.3.255 |
| A6 | 10.147.0.0 | 10.147.1.255 |
| A2 | 10.147.2.0 | 10.147.2.255 |
| A1 | 10.147.3.0 | 10.147.3.127 |
| A3 | 10.147.3.128 | 10.147.3.191 |
| A4 | 10.147.3.192 | 10.147.3.223 |
| A8 | 10.147.3.224 | 10.147.3.255 |
| A5 | 10.147.4.0 | 10.147.4.7 |
| A7 | 10.147.4.8 | 10.147.4.11 |

Setelah mendapatkan hasilnya, dapat disimpulkan bahwa IP awal adalah `10.147.0.0` dan IP akhir adalah `10.147.4.11`. Langkah selanjutnya adalah mencari prefix terbesar. Perbedaan antara IP awal dan IP akhir terjadi mulai oktet ketiga. Kita akan konversi oktet ketiga ke biner (8-bit).

`0` = `00000000`
`4` = `00000100`

Selanjutnya, pada 8-bit tersebut 5 angka pertama memiliki kesamaan. 

`0` = `00000` `0` `00`

`4` = `00000` `1` `00`

Sehingga, kita mendapatkan perhitungan prefix IP, dengan rumus sebagai berikut.
```
Oktet 1 + Oktet 2 + 5 bit oktet 3
```

= 8 + 8 + 5
= 21.

Jadi, prefixnya adalah `/21`.

#### 4) Tabel Hasil
Setelah mendapatkan IP prefixnya, didapat bahwa hasil konfigurasinya sebagai berikut.


| kategori | Nilai |
| --- | --- |
| Network | 10.147.0.0 |
| Prefix | /21 |
| Subnet Mask | 255.255.248.0 |
| IP Range | 10.147.0.0 - 10.147.7.255 |
| Broadcast | 10.147.7.255 |

Untuk tabel lengkapnya dapat dilihat pada link ini.

[Tabel CIDR](https://docs.google.com/spreadsheets/d/1K39cFSm9jZnCzlMsCC4zXNYzNXlhpiq7Xf0Oa_5CWek/edit?usp=sharing)

#### 5) Topologi

![Topologi CIDR](https://github.com/user-attachments/assets/be8e795d-26c7-444b-b7c3-45e4c92a2201)
