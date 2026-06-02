# NetProbe - UDP Tabanli Guvenilir Dosya Aktarimi

Bursa Teknik Universitesi Bilgisayar Aglari donem projesi icin hazirlanan NetProbe, UDP uzerinde uygulama katmaninda guvenilir dosya aktarimi, trafik loglama ve performans analizi yapar.

## Ozellikler

- UDP client/server dosya aktarimi
- Stop-and-Wait guvenilir aktarim
- Sequence number, ACK, timeout ve retransmission
- Paket bazli SHA-256 checksum
- Transfer sonunda FIN + tum dosya SHA-256 dogrulamasi
- Duplicate paket algilama
- Client/server ayri CSV log dosyalari
- Throughput, goodput, RTT, loss rate, retry rate ve completion time metrikleri
- 4 zorunlu deney senaryosu icin otomatik grafik uretimi
- Bonus: Go-Back-N sliding window
- Bonus: UDP Reliable vs TCP karsilastirmasi
- Bonus: terminal dashboard

## Proje Yapisi

```text
src/
  config.py           Parametreler
  protocol.py         DATA / ACK / FIN paketleri ve checksum
  client.py           Stop-and-Wait UDP istemci
  server.py           Stop-and-Wait UDP sunucu
  client_gbn.py       Go-Back-N istemci
  server_gbn.py       Go-Back-N sunucu
  tcp_transfer.py     TCP karsilastirma
  logger.py           CSV olay kaydi
  analysis.py         Metrik ve grafik uretimi
  network_sim.py      Test dosyasi ve kayip/gecikme yardimcilari
  run_experiments.py  Tum deney otomasyonu
data/
  logs/               Client/server ve deney loglari
  received/           Alinan dosyalar
  test_files/         10KB, 100KB, 1MB, 10MB test dosyalari
results/graphs/       Grafikler ve all_experiment_metrics.json
tests/                Unit ve entegrasyon testleri
report/               Teknik rapor daha sonra eklenecek
```

## Kurulum

Python 3.10+ onerilir.

```bash
pip install -r requirements.txt
```

## Hizli Demo

Terminal 1:

```bash
cd src
python server.py received_file.bin
```

Terminal 2:

```bash
cd src
python client.py ..\data\test_files\test_100KB.bin
```

Basarili aktarimda istemci tarafinda `Transfer durumu: BASARILI`, sunucu tarafinda SHA-256 butunluk dogrulamasi gorulur. Alinan dosya `data/received/` altina yazilir.

## Test Dosyalari

```bash
cd src
python network_sim.py
```

Bu komut `data/test_files/` altinda su dosyalari hazirlar:

- `test_10KB.bin`
- `test_100KB.bin`
- `test_1MB.bin`
- `test_10MB.bin`

## Go-Back-N Demo

Terminal 1:

```bash
cd src
python server_gbn.py received_gbn.bin
```

Terminal 2:

```bash
cd src
python client_gbn.py ..\data\test_files\test_100KB.bin
```

## TCP Karsilastirmasi

```bash
cd src
python tcp_transfer.py ..\data\test_files\test_100KB.bin
```

## Tum Deneyleri Calistirma

```bash
cd src
python run_experiments.py
```

Bu komut rubrikteki 4 zorunlu senaryoyu tam kapsamda calistirir:

| Senaryo | Degisen parametreler | Sabitler |
|---|---|---|
| Paket boyutu | 256, 512, 1024, 4096 byte | Timeout=2s, Loss=0% |
| Timeout | 0.5, 1.0, 2.0, 5.0 saniye | PacketSize=1024, Loss=5% |
| Kayip orani | 0%, 5%, 15%, 30% | PacketSize=1024, Timeout=2s |
| Dosya boyutu | 10KB, 100KB, 1MB, 10MB | PacketSize=1024, Timeout=2s, Loss=0% |

Her veri noktasi `N_REPEATS = 3` tekrar ile calistirilir. Ayrintili paket ciktilari terminal yerine `data/logs/experiment_console.log` dosyasina yazilir.

## Uretilen Ciktilar

```text
results/graphs/
  s1_throughput_vs_packetsize.png
  s2_completion_vs_timeout.png
  s3_loss_impact.png
  s4_filesize_impact.png
  saw_vs_gbn_comparison.png
  udp_vs_tcp_comparison.png
  all_experiment_metrics.json
```

Ana metrikler:

- Throughput: retransmission dahil gonderilen toplam byte / sure
- Goodput: basariyla teslim edilen payload byte / sure
- Packet loss rate: timeout sayisi / gonderim sayisi
- Retransmission rate: yeniden gonderim sayisi / paket sayisi
- Average RTT: ACK alinan paketlerde olculen ortalama RTT

## Testler

```bash
python -m compileall -q src tests
python -m pytest -q
```

`pytest` bulunamazsa once kurulum komutunu calistirin:

```bash
pip install -r requirements.txt
```

## Teknik Notlar

- `MAX_RETRIES = 5` zorunlu guvenilirlik kosulu `src/config.py` icinde tanimlidir.
- Stop-and-Wait aktariminda herhangi bir paket basarisiz olursa istemci FIN gondermez ve transfer basarisiz isaretlenir.
- Deney otomasyonu basarisiz runlari sifir degerle grafikleri bozacak sekilde kullanmaz; ayrintilar JSON ve console log icinde saklanir.
- Rapor ve ders sunumu bu teknik paketin disindadir; rapor daha sonra ayrica hazirlanacaktir.

## Grup Bilgisi

Gercek grup uyeleri ve GitHub linki teslimden once buraya eklenmelidir.
