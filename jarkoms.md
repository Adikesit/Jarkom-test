| Name           | NRP        | Kelas     |
| -------------- | ---------- | --------- |
| Arsya Argananta | 5025251059 | C |

> [!IMPORTANT]
> Remember to use the IP prefix allocation provided earlier for this pre-lab assignment (pra-praktikan) and for future ones.
> Access it on [Linktree](https://linktr.ee/jarkom) 

> [!TIP]
> **Due Date : 24th September 2026, 23.00 WIB**


## Put your GNS3 Project files here!

Bagian A: [GitHub](https://github.com/Praktikum-NETICS-2026/jarkom-pra-praktikum-modul1-jarkom-c23/blob/abb1f6bf0239d339be6507c14b49bc25b7fbdb74/pra_1a.gns3project)

Bagian B: [GitHub](https://github.com/Praktikum-NETICS-2026/jarkom-pra-praktikum-modul1-jarkom-c23/blob/333f745daa50febf101f3a0ffa3769e64dcc367b/pra_1b.gns3project)

## Bagian A.6

#### Soal 1

> Buatlah konfigurasi network seperti yang ada pada soal.
> Berikan jawaban konfigurasi network untuk masing-masing netics-pc-1, netics-pc-2 dan netics-pc-4.

> Set up the network configuration as shown in the problem.
> Also, provide the network configuration for each of netics-pc-1, netics-pc-2, and netics-pc-4.

**Answer:**

![topologi](img/topologi.png)

Gambar di atas merupakan konfigurasi jaringan dengan netics-pc-3 berfungsi sebagai _bridge_, sehingga semua _node_ lainnya (netics-pc-1, netics-pc-2, dan netics-pc-4) dapat terhubung secara langsung. Selanjutnya, setiap _node_ dapat dikonfigurasi sebagai berikut:

netics-pc-1:
```
ip addr add 10.177.100.101/24 dev eth0
ip link set eth0 up
```
netics-pc-2:
```
ip addr add 10.177.100.102/24 dev eth0
ip link set eth0 up
```
netics-pc-4:
```
ip addr add 10.177.100.104/24 dev eth0
ip link set eth0 up
```
Pada konfigurasi tersebut, ```10.177``` merupakan **Prefix IP** yang telah disediakan untuk setiap praktikan di mata kuliah jaringan komputer. Hasil konfigurasi dapat dikonfirmasi dengan cara berikut:

Konfirmasi konfigurasi netics_pc-1:
![konfirmasi_ip1](img/ip-1.png)

Konfirmasi konfigurasi netics_pc-2:
![konfirmasi_ip2](img/ip-2.png)

Konfirmasi konfigurasi netics_pc-4:
![konfirmasi_ip4](img/ip-4.png)

#### Soal 2

> Buktikan koneksi antar netics-pc berhasil menggunakan ping dan mtr menunjukkan reply dari seluruh PC tanpa packet loss pada kondisi normal.

> Verify successful connectivity between the netics-pc's using `ping` and `mtr`, ensuring replies are received from all PCs without packet loss under normal conditions.

**Answer:**

Pada tahap ini, pengujian koneksi akan gagal apabila netics-pc-3 (yang berfungsi sebagai _bridge_) yang berada di tengah belum dikonfigurasi untuk meneruskan paket data. Oleh karena itu, perlu dibuat konfigurasi _bridge_ pada netics-pc-3. Berikut merupakan _command_ untuk konfigurasi netics-pc-3:
```
brctl addbr br0
brctl addif br0 eth0
brctl addif br0 eth1
brctl addif br0 eth2
brctl show
ip link set br0 up
```

![check_pc-3](img/check_pc3.png)

Setelah selesai mengonfigurasi netics-pc-3, dapat dilakukan pengujian koneksi. Untuk melakukan pengujian koneksi, ```ping``` dan ```mtr``` dapat digunakan, berikut merupakan hasil pembuktian koneksi antar netics-pc:

***ping:***

ping netics-pc-1:

![ping_1](img/ping-1.png)

ping netics-pc-2:

![ping_2](img/ping-2.png)

(dan seterusnya)

***mtr:***

mtr netics-pc-1 ke netics-pc-2:

![mtr_1-2](img/mtr_1-2.png)

mtr netics-pc-1 ke netics-pc-4:

![mtr_1-4](img/mtr_1-4.png)

Saat melakukan pengujian koneksi menggunakan ```mtr```, terlihat bahwa _reply_ berhasil diterima tanpa terjadi _packet loss_ (0.0%).

#### Soal 3

> Implementasikan tc netem (packet loss) berhasil dan dibuktikan dengan mtr (loss meningkat mendekati 20%).

> The implementation of tc netem (packet loss) was successful and verified using mtr (loss increased to nearly 20%).

**Answer:**

Pengimplementasian _packet loss_ yang mendekati angka 20% dapat dilakukan di netics-pc-3 yang berfungsi sebagai _bridge_ dengan _command_ berikut:
```
tc qdisc replace dev eth0 root netem loss 20%
tc qdisc replace dev eth1 root netem loss 20%
tc qdisc replace dev eth2 root netem loss 20%
```
Hal ini dapat dibuktikan dengan melihat persentase _packet loss_ yang dapat dicek melalui _command_ ```mtr``` di masing-masing _node_.

Pembuktian _packet loss_ dari netics-pc-1 ke netics-pc-2:
![packet_loss_pc1-2](img/ploss1-2.png)

Pembuktian _packet loss_ dari netics-pc-2 ke netics-pc-4:
![packet_loss_pc2-4](img/ploss2-4.png)

(dan seterusnya)

Perlu diperhatikan di sini bahwa persentase _packet loss_ akan mendekati angka 20%, bukan akan selalu bernilai 20%. Hal ini dapat terjadi karena simulasi _packet loss_ dengan ```tc netem``` bekerja berdasarkan probabilitas acak untuk setiap paket yang lewat, bukan menghitung dan membuang paket ke-20 secara pasti. Karena bersifat probabilistik, hasil aktualnya akan berfluktuasi di sekitar angka yang ditentukan, mendekati namun tidak selalu tepat 20% (penjelasan di ambil dari pembahasan pra-praktikum).

Untuk me-_reset_ simulasi _packet loss_ agar kembali menjadi normal, _command_ berikut dapat dijalankan di netics-pc-3:
```
tc qdisc del dev eth0 root
tc qdisc del dev eth1 root
tc qdisc del dev eth2 root
```

#### Soal 4

> Implementasikan tc tbf (throughput limit) berhasil dan dibuktikan dengan iperf3 (bitrate turun mendekati target 50 Mbps).

> The implementation of tc tbf (throughput limit) was successful, as verified by iperf3 (the bitrate dropped to near the 50 Mbps target).

**Answer:**

Pengimplementasian _throughput limit_ mendekati target 50 Mbps dapat dilakukan di netics-pc-3 yang berfungsi sebagai _bridge_ dengan _command_ sebagai berikut:
```
tc qdisc replace dev eth0 root tbf rate 50mbit burst 64k limit 64k
tc qdisc replace dev eth1 root tbf rate 50mbit burst 64k limit 64k
tc qdisc replace dev eth2 root tbf rate 50mbit burst 64k limit 64k
```

Untuk melakukan pembuktian, maka _command_ ```iperf3``` dapat digunakan, dengan melakukan konfigurasi kepada netics-pc-1 untuk dijadikan _server_, dan pada netics-pc-2 dan netics-pc-4 untuk dijadikan _client_.

netics-pc-2 (_client_):
```
iperf3 -c 10.177.100.101 -u -b 100M
```
![pc2_client](img/tp2_client.png)

netics-pc-4 (_client_):
```
iperf3 -c 10.177.100.101 -u -b 100M
```
![pc4_client](img/tp4_client.png)

netics-pc-1 (_server_):
```
iperf3 -s
```
![tp1-2](img/tp1-2_server.png)

![tp1-2-4](img/tp1-2-4_server.png)

Pada gambar-gambar di atas terlihat bahwa _client_ (netics-pc-2 dan netics-pc-4) terus mengirimkan data dengan kecepatan 100 Mbps, namun kecepatan aktual yang diterima oleh server turun menjadi sekitar 48.6 Mbps. Hal ini membuktikan bahwa pembatasan _throughput_ 50 Mbps berhasil, di mana jaringan secara otomatis menurunkan/meng-_drop_ paket data yang melebihi batas kapasitas.

Untuk me-_reset_ _throughput limit_ agar kembali menjadi normal, _command_ berikut dapat dijalankan di netics-pc-3:
```
tc qdisc del dev eth0 root
tc qdisc del dev eth1 root
tc qdisc del dev eth2 root
```

## Bagian B.6

#### Soal 1

> Buatlah konfigurasi seperti pada perintah soal, yaitu 5 netics-pc dan 1 PC capture point terhubung melalui switch.
> Berikan juga jawaban konfigurasi network untuk masing-masing netics-pc.

> Configure the setup as specified in the instructions: 5 netics-pcs and 1 capture point PC connected via a switch.
> Also, provide the network configuration details for each netics-pc.

**Answer:**

![topologi_b](img_b/topo_switch_b.png)

Selanjutnya, seluruh konfigurasi jaringan dapat dilakukan pada seluruh _node_ (netics-pc-1 hingga netics-pc-5). Konfigurasi jaringan dapat dilakukan dengan _command_ berikut:

```
ip addr 10.177.100.101/24 dev eth0
ip link set eth0 up
```
(contoh pada netics-pc-1)
Pada _command_ tersebut, perlu diperhatikan bahwa ```10.177``` merupakan **Prefix IP** dan seluruh _node_ harus berada di satu subnet yang sama (100). Setelah itu

![ip1](img_b/ip1.png)

![ip2](img_b/ip2.png)

![ip3](img_b/ip3.png)

(dan seterusnya)

#### Soal 2

> Buktikan koneksi antar netics-pc berhasil menggunakan ping dan mtr menunjukkan reply dari seluruh PC tanpa packet loss pada kondisi normal.

> Verify the connectivity between the netics-pc's using `ping` and `mtr`, ensuring replies are received from all PCs without packet loss under normal conditions.

**Answer:**

Untuk membuktikan bahwa antar _node_ telah terhubung, _command_ `ping` dan `mtr` dapat digunakan. Berikut merupakan _screenshot_ hasil koneksi antar _node_:

`ping`:

![tes_ping1](img_b/tes_ping1.png)

![tes_ping2](img_b/tes_ping2.png)

`mtr`:

![tes_mtr1-2](img_b/mtr1-2.png)

![tes_mtr1-3](img_b/mtr1-3.png)

![tes_mtr1-4](img_b/mtr1-4.png)

![tes_mtr1-5](img_b/mtr1-5.png)

#### Soal 3

> Jalankan termshark di netics-pc-6, lalu buat traffic ping antara netics-pc-1 dan netics-pc-2. Catat apakah traffic tersebut ikut tercapture di netics-pc-6

> Run termshark on netics-pc-6, then generate ping traffic between netics-pc-1 and netics-pc-2. Note whether that traffic is also captured on netics-pc-6.

**Answer:**

Untuk menjalankan _termshark_ di netics-pc-6, maka dalam kasus ini dapat menggunakan _command_:

```
ip link set eth0 up
termshark -i eth0
```
![set_pc-6](img_b/set6.png)

Selanjutnya, akan didapatkan UI termshark sebagai berikut:

![ui_termshark](img_b/tshark6.png)

Setelah termshark siap, maka dapat dilakukan _traffic ping_ antara netics-pc-1 dengan netics-pc-2.

![switch_tshark6](img_b/switch_ping6.png)

Berdasarkan gambar di atas, terlihat bahwa _traffic ping_ antara netics-pc-1 dan netics-pc-2 tidak ikut tertangkap di netics-pc-6. Hal ini dapat terjadi karena cara kerja _switch_ sangat pintar. _Switch_ secara otomatis dapat mengenali dan mencatat identitas (_MAC Address_) dari setiap _node_ yang terhubung kepadanya, lalu menyimpannya ke dalam sebuah daftar memori bernama _MAC Address Table_. Hal ini dapat mencegah terjadinya kebocoran _frame_.

#### Soal 4

> Ganti node switch dengan hub, ulangi capture dari netics-pc-6 dengan skenario ping yang sama. Catat apakah traffic tersebut ikut tercapture di netics-pc-6

> Replace the switch node with a hub, and repeat the capture from netics-pc-6 using the same ping scenario. Note whether that traffic is also captured on netics-pc-6.

**Answer:**

Berikut merupakan topologi terbaru jika _node switch_ diubah dengan _hub_:

![topologi_hub](img_b/topo_hub_b.png)

Selanjutnya, dapat dilakukan langkah-langkah yang sama seperti nomor tiga:

![hub](img_b/hub_ping6.png)

Berdasarkan gambar di atas, terlihat bahwa _traffic ping_ antara netics-pc-1 dan netics-pc-2 ikut tertangkap di netics-pc-6. Hal ini dapat terjadi karena _node hub_ tidak memiliki kemampuan yang sama seperti _switch_. _Hub_ tidak dapat secara otomatis mengenali dan mencatat _MAC Address_ dari setiap _node_ yang terhubung kepadanya, sehingga semua data yang masuk dari netics-pc-1 akan disebarluaskan ke semua _port_ yang terhubung, termasuk netics-pc-6.

#### Soal 5

> Bandingkan kedua hasil capture (switch vs hub), dan tuliskan kesimpulan mengenai broadcast domain dan collision domain berdasarkan hasil observasi sendiri

> Compare the two captures (switch vs. hub) and write a conclusion regarding broadcast domains and collision domains based on your own observations.

**Answer:**

Berdasarkan pengamatan dan pembahasan yang telah dijelaskan sebelumnya, terlihat bahwa _traffic ping_ antara netics-pc-1 dengan netics-pc-2 dapat ditangkap oleh netics-pc-6 saat menggunakan _hub_, namun tidak dapat ditangkap saat menggunakan _switch_. Hal ini terjadi karena _switch_ secara otomatis dapat mengenali dan mencatat _MAC Address_ dari setiap _node_ yang terhubung kepadanya, lalu menyimpannya ke dalam sebuah daftar memori bernama _MAC Address Table_. Hal ini dapat mencegah terjadinya kebocoran _frame_. Berbeda dengan _hub_ yang tidak memiliki kemampuan seperti _switch_. Selain kemampuan yang berbeda, pada _hub_, seluruh perangkat juga berada dalam satu _collision domain_ sekaligus _satu broadcast domain yang sama_, sehingga setiap _frame_, diterima oleh semua perangkat yang terhubung.

_Broadcast domain_: wilayah di mana semua _node_ yang terhubung dapat menerima pesan _broadcast_ yang dikirim oleh suatu _node_ pada segmen yang sama.
_Collision domain_: wilayah di mana dua atau lebih _node_ dapat mengirimkan pesan secara bersamaan, namun berisiko menyebabkan data saling bertabrakan.

#### Soal 6

> Tambahkan satu netics-pc ketujuh yang terhubung ke port terpisah pada switch/hub yang sama. Analisis apa kah traffic broadcast seperti ARP request tetap diterima oleh netisc-pc ketujuh tersebut pada kedua skenario (switch dan hub)
> Jelaskan secara singkat mengapa demikian

> Add a seventh netics-pc connected to a separate port on the same switch or hub. Analyze whether broadcast traffic, such as ARP requests, is still received by this seventh netics-pc in both scenarios (switch and hub).
> Briefly explain why this is the case.

**Answer:**

![switch_ping7](img_b/switch_ping7.png)

![hub_ping7](img_b/arp_ping7.png)

Berdasarkan kedua gambar di atas, terlihat bahwa paket `ARP Request` dapat diterima oleh netics-pc-7, baik saat jaringan menggunakan _hub_ maupun _switch_. Hal ini terjadi karena `ARP Request` merupakan jenis _broadcast traffic_, sehingga paket tersebut akan diteruskan ke seluruh _node_ yang berada dalam satu _broadcast domain_ yang sama.

*_Note_: agar paket `ARP Request` dapat dikirim ulang dan tertangkap di netics-pc-7, _cache_ ARP pada netics-pc-1 harus dikosongkan terlebih dahulu (di sini saya lakukan dengan _restart_/_reload_ node netics-pc-1). Tanpa mengosongkan cache, netics-pc-1 akan langsung mengirimkan paket _unicast_ (ICMP) berdasarkan informasi _MAC Address_ yang sudah tersimpan sebelumnya, sehingga `ARP Request` tidak akan terkirim.
