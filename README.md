<h1 align="center">Ödüllü NuLink Horus Testneti Kurulum Rehberi

## Merhabalar, bugün NuLink testnetine katılıyor olacağız. Testnet iki aşamalı olacak ve ödüllü bir testnet olduğu belirtilmiş. Sağ üstten yıldızlayıp forklamayı unutmayalım.

![Fcl-LR-WQAIbH3x](https://user-images.githubusercontent.com/101462877/190577705-86cd74be-26f2-4588-a20d-bde0aaffdf2a.jpg)



## Aşamaların detaylarını ve ödül açıklamasını aşağıdaki görselde bulabilirsiniz. Sorularınız olursa: [LossNode Chat](https://t.me/LossNode)

## Ayrıca [NuLink Türkiye](https://t.me/NuLink_Turkey) Telegram kanalına da katılabilirsiniz.

![image](https://user-images.githubusercontent.com/101462877/190577423-3d3119c4-b389-4845-8a5c-97d6bc0b3699.png)

## Yukarıdaki açıklamada gördüğünüz üzere testnet için feedback oldukça önemli, kurulum yaptıktan sonra feedback vermeyi unutmayın.

Detaylı Bilgi: [Testnet Duyurusu](https://www.nulink.org/blog-posts/nulink-testnet-announcement)

Feedback Linki: https://bit.ly/NuLinkFeedback

## Minimum sistem gereksinimleri:
NODE TİPİ | CPU     | RAM      | SSD     |
| ------------- | ------------- | ------------- | -------- |
| Testnet | 2          | 4         | 30  |


## NuLink için önemli linkler:
- [Website](https://www.nulink.org/)
- [Explorer(ımsı)](https://test-staking.nulink.org)
- [Twitter](https://twitter.com/NuLink_)
- [Discord](https://discord.gg/Vr5AGBbjBe)

# Kuruluma başlayalım. İlk olarak sunucumuza gerekli güncellemeleri ve kurulumları yapıyoruz.

```
sudo su
cd
sudo ufw allow 9151
sudo apt update && sudo apt upgrade -y
sudo apt-get -y install libssl-dev && apt-get -y install cmake build-essential git wget jq make gcc
```
```
wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.10.24-972007a5.tar.gz
tar -xvzf geth-linux-amd64-1.10.24-972007a5.tar.gz
cd geth-linux-amd64-1.10.24-972007a5/
./geth account new --keystore ./keystore
```

![image](https://user-images.githubusercontent.com/101462877/190579879-4716148c-c8cc-4e86-adae-6a2962d93420.png)

Bu kısımda bizden şifre oluşturmamızı isteyecek, oluşturduğumuz şifreyi yazıyoruz. (Şifreyi yazarken ekranda gözükmese de algılar, şifrenizi doğru girdiğinize emin olun)

![image](https://user-images.githubusercontent.com/101462877/190581008-01771cc0-4831-4ee4-a364-a95c22c52529.png)

Repeat Password kısmına da aynı şifreyi girdikten sonra karşımıza böyle bir şey çıkacak, public address'i ve path of the secret key'i kopyalayıp bir yere not ediyoruz.

# Gerekli bilgileri not ettikten sonra kuruluma docker ile devam ediyoruz.

Bu kısımlarda yüklemeler uzun sürebilir, bekleyin.

```
cd $HOME
sudo apt install docker.io -y
```
```
sudo systemctl enable --now docker
docker pull nulink/nulink:latest
cd $HOME
mkdir NuLink
```

Aşağıdaki komut için değiştirmeniz gereken kısım, daha önce bir yere not ettiğimiz `path of the secret key file` kısmı. Yani görselde gördüğünüz sarı kutunun tamamı.
![image](https://user-images.githubusercontent.com/101462877/190584284-a8de08b1-2b79-4803-bd7a-950957312e7f.png)


```
cp <KENDİ PATH OF SECRET KEY FILE'INIZ> /root/NuLink
```
Ardından kopyaladığımız dosyaya izin veriyoruz.
```
chmod -R 777 /root/NuLink
```

# Node yapılandırması için kullanacağımız değişkenleri ayarlıyoruz. Ben kolaylık olması için şifrelerin hepsini aynı yapıyorum.

```
export NULINK_KEYSTORE_PASSWORD=<EN AZ 8 HANELİ BİR ŞİFRE>

export NULINK_OPERATOR_ETH_PASSWORD=<İLK BAŞTA OLUŞTURDUĞUMUZ ŞİFRE>
```

# Ardından aşağıdaki komut ile node yapılandırması gerçekleştiriyoruz. Değiştirmeniz gereken kısımları komutun altında belirttim.
```
docker run -it --rm \
-p 9151:9151 \
-v /root/NuLink:/code \
-v /root/NuLink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
nulink/nulink nulink ursula init \
--signer keystore:///code/<PATH OF THE SECRET KEY'İN UTC'DEN İTİBAREN SONUNA KADAR OLAN KISMI> \
--eth-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--network horus \
--payment-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--payment-network bsc_testnet \
--operator-address <PUBLIC ADDRESS'INIZ> \
--max-gas-price 100
```
- `<PATH OF THE SECRET KEY'İN UTC'DEN İTİBAREN SONUNA KADAR OLAN KISMI>`: UTC--2022-09-16T07-20-30.030086XXXXX--4f31XXXX96bcf5XXXXXXXXXXXXX kısmı. Aşağıdaki görselde sarı kutunun içeriği yani.

![image](https://user-images.githubusercontent.com/101462877/190588180-b6dbb2aa-696d-4aa7-bac6-36d6649695ad.png)

- `<PUBLIC ADDRESS'INIZ>`: En başta password oluşturduktan sonra karşınıza çıkan ve not ettiğiniz Public Address. Aşağıdaki görselde sarı kutunun içeriği.

![image](https://user-images.githubusercontent.com/101462877/190588620-57ce840d-f7d7-4560-b383-3867b2988103.png)


Düzenlemeleri yaptıktan sonra komutun tamamını terminale yapıştırıyoruz.

# Aşağıdaki görselde `1` ile gösterdiğim kısımda y'ye bastıktan sonra karşımıza böyle bir ekran çıkıyor, sarı kutuyla gösterdiğim kısımdaki bize verdiği kelimeleri not ediyoruz. Kelimeleri not ettikten sonra `2` ile gösterdiğim kısımda da y'ye basıyoruz.
![image](https://user-images.githubusercontent.com/101462877/190589761-64dd9d94-854b-4b11-bf1b-ea39147651ef.png)

# `2` ile gösterdiğim kısımda da y'ye bastıktan sonra bizden not aldığımız kelimeleri girmemizi istiyor. Not aldığımız kelimelerin tamamını burada giriyoruz. 

![image](https://user-images.githubusercontent.com/101462877/190590627-6200fe33-a41e-4658-9b8d-b2f4ba48628a.png)

# Bir süre bekledikten sonra aşağıdaki gibi bir çıktı alacağız.

![image](https://user-images.githubusercontent.com/101462877/190591572-157fcbdc-6176-4e76-8ee4-354673b65af7.png)

# Sonrasında aşağıdaki komutu direkt yapıştırıyoruz.

```
docker run --restart on-failure -d \
--name ursula \
-p 9151:9151 \
-v /root/NuLink:/code \
-v /root/NuLink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
-e NULINK_OPERATOR_ETH_PASSWORD \
nulink/nulink nulink ursula run --no-block-until-ready
```

Bu komutun ardından bize aşağıda sarı okla gösterdiğim çıktıyı veriyor.

![image](https://user-images.githubusercontent.com/101462877/190592327-a2a4de59-d101-4f60-8eb3-5d0b54b0c7aa.png)

# Logları kontrol etmek için screen yükleyip screen açıyoruz ve logları kontrol ediyoruz.

```
apt install screen -y
```
```
screen -S NuLinklogs
docker logs -f ursula
```
![image](https://user-images.githubusercontent.com/101462877/190593031-73cf88d4-3e08-4649-9362-f631ba0ffa09.png)

Yukarıdaki gibi bir çıktı kurulumu doğru yaptığımızı gösterir.

# NODE KURULUMU BİTTİ FAKAT DAPP ÜZERİNDE STAKING İÇİN AŞAĞIDAKİ İŞLEMLER İLE DEVAM EDİYORUZ.

Bu kısımda metamask üzerinde BSC Testnet ağı lazım olacak. Metamask'ınızde BSC Testnet ağı ekli değilse https://chainlist.org 'a giderek ekleyebilirsiniz.

![image](https://user-images.githubusercontent.com/101462877/190594305-179f82e7-2f43-40e5-873a-2d3e35269edf.png)

# Ardından https://testnet.binance.org/faucet-smart adresine gidiyoruz ve test BNB alıyoruz.

![image](https://user-images.githubusercontent.com/101462877/190595471-05f0602c-e831-4e74-91a0-dfe8da4ece7d.png)


# BSC Testneti metamask'e ekledikten ve test BNB aldıktan sonra https://test-staking.nulink.org/faucet adresine gidiyoruz ve cüzdan bağlayıp faucet'e tıklıyoruz.

![image](https://user-images.githubusercontent.com/101462877/190596016-dea51928-0079-4a11-a446-4fb714b792c0.png)

# Test tokenları başarıyla aldıktan sonra `stake` kısmına gelerek test tokenları stake ediyoruz.

![image](https://user-images.githubusercontent.com/101462877/190596306-6222875f-33a7-4b2d-a79d-85725b687995.png)

![image](https://user-images.githubusercontent.com/101462877/190596405-e10d4373-8e94-4ac6-8bfe-935152085e92.png)

# Tokenları stake ettikten sonra `Bond Worker`a tıklayıp tokenları bond ediyoruz.

![image](https://user-images.githubusercontent.com/101462877/190596602-9d39b53e-a248-45f8-af10-c12680cd2f1d.png)

![image](https://user-images.githubusercontent.com/101462877/190596979-2de3561f-c816-44bd-a7f5-01dcf7be4a36.png)

Bu kısımda doldurmanız gereken yerler;

- `Worker Address`: Aşağıda sarı kutuyla gösterdiğim Public Address'iniz.

![image](https://user-images.githubusercontent.com/101462877/190597172-0e241a3f-8574-4995-a15e-75a4b1e77abf.png)

- `Node URI`: https://xxxxxxxx:9151  xxxxxxxxxxx kısımları Sunucu IP'niz. Örneğin: https://123.456.789.012:9151


# Ardından `Node List` kısmına gelip node'unuzun aktif olup olmadığına bakabilirsiniz. Aşağıdaki gibi `online` gözüküyorsa doğru yapmışsınız demektir.

![image](https://user-images.githubusercontent.com/101462877/190598135-72403549-3272-41b3-881d-59c5813bd6f6.png)

# Node'unuzu kurup token bond ettikten sonra log'lar aşağıdaki gibi olacaktır. Terminal üzerinde screen'den çıkmak için önce Ctrl + A 'ya basın, 1-2 saniye sonra D'ye basın.

![image](https://user-images.githubusercontent.com/101462877/190599242-3f083159-bf70-44f1-9aff-d644fa9ca2ac.png)


# FEEDBACK VERMEYİ UNUTMAYIN. KATILAN ANCAK FEEDBACK VERMEYENLERİN ÖDÜL ALMAYACAĞI BELİRTİLMİŞ.

![image](https://user-images.githubusercontent.com/101462877/190599726-c6a5a506-735b-4fa3-a597-1f8bf0e54d0b.png)


Feedback Linki: https://bit.ly/NuLinkFeedback


# ÖDÜLLÜ TESTNET İÇİN İLK AŞAMA BU KADARDI. HERHANGİ BİR SORUNUZDA AŞAĞIDAKİ TELEGRAM ADRESLERİNDE BİZE ULAŞABİLİRSİNİZ.

- LossNode Telegram: https://t.me/LossNode

- NuLink Türkiye: https://t.me/NuLink_Turkey

# Son olarak, NuLink testnet rehberini ondan ilham alarak hazırladığım [BlackOwl](https://twitter.com/brsbtc)'a teşekkür ediyorum. 

  
# Ayrıca orijinal kurulum dokümantasyonuna bakmak isterseniz: [NuLink Docs](https://docs.nulink.org/products/testnet)
