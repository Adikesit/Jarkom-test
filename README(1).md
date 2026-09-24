| Name           | NRP        | Kelas     |
| ---------------| -----------|  ---------|
|Muhammad Adinata Parikesit|5025251041|B|

> [!IMPORTANT]
> Remember to use the IP prefix allocation provided earlier for this pre-lab assignment (pra-praktikan) and for future ones.
> Access it on [Linktree](https://linktr.ee/jarkom) 

> [!TIP]
> **Due Date : 24th September 2026, 23.00 WIB**


## Put your GNS3 Project files here!

`Put file URL here`

## Bagian A.6

#### Soal 1

> Buatlah konfigurasi network seperti yang ada pada soal.
> Berikan jawaban konfigurasi network untuk masing-masing netics-pc-1, netics-pc-2 dan netics-pc-4.

> Configure the setup as specified in the instructions: 5 netics-pcs and 1 capture point PC connected via a switch.
> Also, provide the network configuration details for each netics-pc.

**Answer:**

<img width="2437" height="1536" alt="image" src="https://github.com/user-attachments/assets/867edf58-c15a-4f64-ada6-8b847854ed5f" />


#### Soal 2

> Buktikan koneksi antar netics-pc berhasil menggunakan ping dan mtr menunjukkan reply dari seluruh PC tanpa packet loss pada kondisi normal.

> Verify successful connectivity between the netics-pc's using `ping` and `mtr`, ensuring replies are received from all PCs without packet loss under normal conditions.

**Answer:**


<img width="1224" height="765" alt="image" src="https://github.com/user-attachments/assets/0e3e9ac9-e76b-4e30-90b8-21ad5f219d77" />
<img width="2445" height="1556" alt="image" src="https://github.com/user-attachments/assets/4fafc43b-19b4-4442-923a-2452bf84caa6" />



#### Soal 3

> Implementasikan tc netem (packet loss) berhasil dan dibuktikan dengan mtr (loss meningkat mendekati 20%).

> The implementation of tc netem (packet loss) was successful and verified using mtr (loss increased to nearly 20%).

**Answer:**


<img width="1080" height="648" alt="image" src="https://github.com/user-attachments/assets/9855b5be-1dd6-42e3-80d9-e531feb6a5dc" />
<img width="2456" height="1549" alt="image" src="https://github.com/user-attachments/assets/85d15299-1ae2-4c82-b9c5-9da48cff5b23" />



#### Soal 4

> Implementasikan tc tbf (throughput limit) berhasil dan dibuktikan dengan iperf3 (bitrate turun mendekati target 50 Mbps).

> The implementation of tc tbf (throughput limit) was successful, as verified by iperf3 (the bitrate dropped to near the 50 Mbps target).

**Answer:**

<img width="2445" height="1551" alt="image" src="https://github.com/user-attachments/assets/20b51795-c9a6-4c6c-ba49-563f540604a8" />

## Bagian B.6

#### Soal 1

> Buatlah konfigurasi seperti pada perintah soal, yaitu 5 netics-pc dan 1 PC capture point terhubung melalui switch.
> Berikan juga jawaban konfigurasi network untuk masing-masing netics-pc.

> Configure the setup as specified in the instructions: 5 netics-pcs and 1 capture point PC connected via a switch.
> Also, provide the network configuration details for each netics-pc.

**Answer:**

<img width="2557" height="1594" alt="image" src="https://github.com/user-attachments/assets/9fadc4b3-0acf-478d-b27f-15d5fc0dc0f7" />


#### Soal 2

> Buktikan koneksi antar netics-pc berhasil menggunakan ping dan mtr menunjukkan reply dari seluruh PC tanpa packet loss pada kondisi normal.

> Verify the connectivity between the netics-pc's using `ping` and `mtr`, ensuring replies are received from all PCs without packet loss under normal conditions.

**Answer:**

<img width="2557" height="1598" alt="image" src="https://github.com/user-attachments/assets/f6bea23c-4c1a-47a3-8aeb-d9cf674886ea" />

#### Soal 3

> Jalankan termshark di netics-pc-6, lalu buat traffic ping antara netics-pc-1 dan netics-pc-2. Catat apakah traffic tersebut ikut tercapture di netics-pc-6

> Run termshark on netics-pc-6, then generate ping traffic between netics-pc-1 and netics-pc-2. Note whether that traffic is also captured on netics-pc-6.

**Answer:**

<img width="2556" height="1598" alt="image" src="https://github.com/user-attachments/assets/2de2c5d0-33cf-4831-ab21-8ebb033921d7" />


#### Soal 4

> Ganti node switch dengan hub, ulangi capture dari netics-pc-6 dengan skenario ping yang sama. Catat apakah traffic tersebut ikut tercapture di netics-pc-6

> Replace the switch node with a hub, and repeat the capture from netics-pc-6 using the same ping scenario. Note whether that traffic is also captured on netics-pc-6.

**Answer:**

<img width="2445" height="1566" alt="image" src="https://github.com/user-attachments/assets/44876b8a-2ef2-46c4-aa09-7e23d4cee885" />


#### Soal 5

> Bandingkan kedua hasil capture (switch vs hub), dan tuliskan kesimpulan mengenai broadcast domain dan collision domain berdasarkan hasil observasi sendiri

> Compare the two captures (switch vs. hub) and write a conclusion regarding broadcast domains and collision domains based on your own observations.

**Answer:**

<img width="2445" height="1566" alt="image" src="https://github.com/user-attachments/assets/9cf58766-8f0e-4b9b-830f-924b00e43258" />
<img width="2556" height="1598" alt="image" src="https://github.com/user-attachments/assets/2de2c5d0-33cf-4831-ab21-8ebb033921d7" />

``

  Berdasarkan hasil yang ada da terbukti bahwa traffic broadcast seperti ARP request tetap di terima apabila kita menggunakan HUB dan sebaliknya untuk Switc traffic broadcast seperti ARP request tidak terbaca di netics-pc-6
``

#### Soal 6

> Tambahkan satu netics-pc ketujuh yang terhubung ke port terpisah pada switch/hub yang sama. Analisis apa kah traffic broadcast seperti ARP request tetap diterima oleh netisc-pc ketujuh tersebut pada kedua skenario (switch dan hub)
> Jelaskan secara singkat mengapa demikian

> Add a seventh netics-pc connected to a separate port on the same switch or hub. Analyze whether broadcast traffic, such as ARP requests, is still received by this seventh netics-pc in both scenarios (switch and hub).
> Briefly explain why this is the case.

**Answer:**
<img width="2479" height="1598" alt="image" src="https://github.com/user-attachments/assets/5e72a461-58f5-4536-a9ca-c3574d9ec05d" />
<img width="2484" height="1596" alt="image" src="https://github.com/user-attachments/assets/a84d738f-3d58-497e-b4be-0a9af4c78971" />


```
put your answer here (or additionally screenshot)
```
