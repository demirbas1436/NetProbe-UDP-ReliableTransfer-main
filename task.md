# NetProbe - Teknik Teslim Takibi

## Durum

Hedef: rapor ve sunum haricindeki teknik rubrik puanlarini en ust seviyeye tasimak.

## Temel Sistem

- [x] UDP client/server dosya aktarimi
- [x] Stop-and-Wait aktarim
- [x] DATA / ACK / FIN paket yapisi
- [x] SHA-256 paket checksum
- [x] Transfer sonu dosya butunluk kontrolu

## Guvenilir Aktarim

- [x] Sequence number
- [x] ACK kontrolu
- [x] Timeout
- [x] Retransmission
- [x] `MAX_RETRIES = 5`
- [x] Duplicate paket algilama
- [x] Basarisiz paket varsa transferi basarisiz isaretleme

## Trafik Izleme ve Analiz

- [x] Client/server ayri CSV loglari
- [x] SENT, ACK_RECEIVED, TIMEOUT, RETRANSMIT, FAILED, COMPLETE olaylari
- [x] Throughput, goodput, RTT, loss rate, retransmission rate
- [x] Grafik uretimi

## Deneyler

- [x] Senaryo 1: Paket boyutu 256, 512, 1024, 4096
- [x] Senaryo 2: Timeout 0.5, 1.0, 2.0, 5.0
- [x] Senaryo 3: Kayip orani 0%, 5%, 15%, 30%
- [x] Senaryo 4: Dosya boyutu 10KB, 100KB, 1MB, 10MB
- [x] Her ana veri noktasi icin 3 tekrar
- [x] Basarisiz runlari sifir metrik olarak grafikte kullanmama

## Bonuslar

- [x] Go-Back-N sliding window
- [x] SAW vs GBN karsilastirma grafigi
- [x] UDP Reliable vs TCP karsilastirma grafigi
- [x] Terminal dashboard

## Dogrulama Komutlari

```bash
pip install -r requirements.txt
python -m compileall -q src tests
python -m pytest -q
cd src
python run_experiments.py
```

## Kapsam Disi

- [ ] Teknik rapor daha sonra yazilacak
- [ ] Ders sunumu sinifta yapilacak
- [ ] Gercek GitHub linki ve grup bilgisi teslimden once README'ye eklenecek
