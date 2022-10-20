<h1 align="center">Ödüllü NuLink Testneti Güncelleme Rehberi

## Merhabalar, bugün daha öncesinde katıldığımız NuLink testneti için güncelleme yapıyor olacağız. Sağ üstten yıldızlayıp forklamayı unutmayalım. Sorularınız olursa: [LossNode Chat](https://t.me/LossNode)
  
Ayrıca NuLink Türkiye Telegram: https://t.me/NuLink_Turkey

## Node'u durdurun.

```
docker kill ursula
docker rm ursula
```

## Şifreleri export edin.
```
export NULINK_KEYSTORE_PASSWORD=<DAHA ÖNCE OLUŞTURDUĞUNUZ ŞİFRE>

export NULINK_OPERATOR_ETH_PASSWORD=<DAHA ÖNCE OLUŞTURDUĞUNUZ ŞİFRE>
```

## Güncel image'ı çekin.
```
docker pull nulink/nulink:latest
```

## Node başlatın.
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
