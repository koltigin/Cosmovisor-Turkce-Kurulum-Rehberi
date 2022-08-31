# Cosmovisor-Turkce-Kurulum-Rehberi
Cosmos SDK Cosmovisor Türkçe Kurulum Rehberi

* Bu repo [Cosmovisor](https://github.com/cosmos/cosmos-sdk/tree/master/cosmovisor) kaynağına göre hazırlanmıştır.

### Cosmovisor Kurulumu
```shell
git clone https://github.com/cosmos/cosmos-sdk
cd cosmos-sdk
git checkout v0.42.7
make cosmovisor
cp cosmovisor/cosmovisor $GOPATH/bin/cosmovisor
cd $HOME
```

## Değişkenleri Ayarlama
- `sourced` bu bölüme hangi node için kurulum yapıyorsanız onun binary dosyasını yazıyorsunuz. Örn: gaiad, kujirad, palomad, rebusd, seid, stafihubd, teritorid, umeed gibi. 
- `.source` yazan yeri node'un sistem dosyalarının kurulu olduğu dizini yazıyoruz. Örn: .gaiad, .kujirad, .palomad, .rebusd, .seid, .stafihubd, .teritorid, .umeed gibi.
```shell
echo "# Cosmovisor Kurulumu" >> ~/.profile
echo "export DAEMON_NAME=sourced" >> ~/.profile
echo "export DAEMON_HOME=$HOME/.source" >> ~/.profile
echo "export DAEMON_RESTART_AFTER_UPGRADE=true" >> ~/.profile
source ~/.profile
```

## Node İsmini Kontrol Etme
```shell
echo $DAEMON_NAME
```

## Cosmovisor Klasörlerini Yapılandırma
```shell
mkdir -p $DAEMON_HOME/cosmovisor/genesis/bin
mkdir -p $DAEMON_HOME/cosmovisor/upgrades
```

## Binary Dosyalarını Cosmovisor Altına Taşıma
`sourced` yani node binary dosyası. Örn: gaiad, kujirad, palomad, rebusd, seid, stafihubd, teritorid, umeed gibi. 
```shell
cp /home/root/go/bin/sourced $DAEMON_HOME/cosmovisor/genesis/bin
```

## Servis Dosyasını Yapılandırma
Aşağıdaki kod ile node'un servis dosyasını açıyoruz.
- `sourced` Örn: gaiad, kujirad, palomad, rebusd, seid, stafihubd, teritorid, umeed gibi.
```shell
sudo nano /etc/systemd/system/sourced.service
```
Açılan dosyanın içerisini `CTRL K`'ya basarak siliyoruz ve aşağıdaki kodu düzenleyerek yapıştırıyoruz. Ardından` CTRL X, Y, Enter` tuşlayarak kaydediyoruz.
- `sourced` Örn: gaiad, kujirad, palomad, rebusd, seid, stafihubd, teritorid, umeed gibi.
- `.source` Örn: gaiad, kujirad, palomad, rebusd, seid, stafihubd, teritorid, umeed gibi.
```shell
[Unit]
Description=cosmovisor                    # Bu bölüme kuracağınız node'a göre isim verebilirsiniz. Örn: StaFiHub Daemon - Cosmovisor
After=network-online.target
[Service]
User=root
ExecStart=/home/root/go/bin/cosmovisor start
Restart=always
RestartSec=3
LimitNOFILE=4096
Environment="DAEMON_NAME=sourced"         # Bu bölüm düzenlenecek
Environment="DAEMON_HOME=$HOME/.source    # Bu bölüm düzenlenecek
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="DAEMON_LOG_BUFFER_SIZE=512"
[Install]
WantedBy=multi-user.target
```

## Sistemi Başlatma
- `sourced` Örn: gaiad, kujirad, palomad, rebusd, seid, stafihubd, teritorid, umeed gibi.
```shell
sudo -S systemctl daemon-reload
sudo -S systemctl enable sourced
sudo systemctl start sourced
```

## Node Durumunu Kontrol Etme
- `sourced` Örn: gaiad, kujirad, palomad, rebusd, seid, stafihubd, teritorid, umeed gibi.
```shell
sudo systemctl status sourced
```

## Logları Kontrol Etme
- `sourced` Örn: gaiad, kujirad, palomad, rebusd, seid, stafihubd, teritorid, umeed gibi.
```shell
journalctl -fu sourced -o cat
```
