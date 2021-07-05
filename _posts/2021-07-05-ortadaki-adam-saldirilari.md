---
layout: post
title: "Ortadaki Adam Saldırıları ve Açık Ağlarda Güvenlik Sağlamak"
author: efe
categories: [ Lifestyle ]
image: ![image](https://user-images.githubusercontent.com/86926775/124506635-c1410080-ddd4-11eb-8145-e5531675e0fd.png)
beforetoc: ""
toc: false
---
İlerleyen teknoloji bizi hayatlarımızın çoğunu evde bilgisayar başında geçirmeye
sürükledi ancak bunu her gün yapamayacağımıza karar verdiğimizde taşınabilir
bilgisayarlar (Dizüstü bilgisayarlar, tabletler, mobil telefonlar ve daha fazlası)
ve evlerimiz dışında bilgisayar kullanabileceğimiz alanlar oluşturmaya başladık.

Zaman ilerledikçe kablosuz ağlar gibi pek çok teknoloji barındıran bu alanları
evlerimizden daha sık kullanır olduk.

Halka açık alanlarda bulunan ve hepimizin kullandığı kablosuz ağlar aslında
sandığımız kadar güvenli değil. Açık ağlar ne kadar güvensiz olsa da kendimizi
korumak bir o kadar kolay. Bu yazımızda halka açık alanlarda karşılaşabileceğimiz
ortadaki adam saldırılarından ve kendimizi bu saldırılardan nasıl koruyabileceğimizden
bahsedeceğiz.

Ortadaki adam saldırıları, bilgisayar korsanları tarafından sıkça kullanılan
popüler bir saldırı türüdür. Temel olarak ağdaki iletişimi günlük hayata
uyarlamamız gerekirse, bu iletişim iki insanın konuşmasına benzer. Şimdi buna
bir senaryo ile bakalım.

Ali ve Ela adındaki iki kişi konuşuyor olsun. Kötü amaçlı kişi, bu konuşmayı iki
amaçla bölmek isteyebilir. İlk amaç, Ali ve Ela’nin aralarında geçen konuşmayı
dinlemek ve bilgi edinmek, ikinci amaç ise saldırganın Ali’in kurduğu cümleyi
Ela’ya ulaşmadan kesip değiştirerek Ela’nın farklı bir cümle duymasını sağlamak
olabilir.

## Ağların Temel İşleyişi
Ortadaki adam saldırılarının detaylarına inmeden önce yerel ağların işleyişi
hakkında biraz fikir sahibi olmalıyız.

Yerel bir ağda iki bilgisayar olduğunu düşünelim. Bu bilgisayarlar internetle
iletişim kurmak için bir aracı olarak modemi kullanırlar. Modemin bu iki cihazı
birbirinden ayırt edebilmesi için cihazlara özel (Ağ içi) IP (İnternet protokolü)
adresi vermesi gerekir. Bunu sağlamak için bilgisayarlar modemden IP adresi
isterler. İnternete bağlanırken telefonlarınızda gördüğünüz “IP adresi alınıyor”
yazısı bu yüzden çıkmaktadır. Ağdaki cihazlara IP adresleri verildikten sonra
cihazlar aralarında iletişim kurabilir hale gelmiş olur.

Cihazların aralarında iletişim kurması için birbirlerini tanıması gerekmektedir.
Artık ağdaki her cihaza modem tarafından IP adresleri verildiğine göre iletişimi
sağlamak için tek bir engel kalır. Cihazlar modemi, modem de cihazları tanıyor
haldedir ancak cihazlar birbirlerini tanımamaktadırlar. Bu sorunun nasıl
aşıldığını bir örnek ile anlatalım. Ali, içinde Ela’nin de bulunduğu 20 kişilik bir
sınıfta Ela’yi arıyor olsun. Bu durumda zaten ismini bildiği için sınıfta “Ben
Ali. İsmi Ela olan kişi beni bulsun.” diyerek bağırdığında Ela’yi bulabilir. Bu
örnekte “Ela” ismi eşsizdir ve yerel ağda ulaşılmaya çalışılan cihazın IP adresi
gibi düşünülebilir.

Cihazların yerel ağda birbirlerini tanımaları için gereken ve yukarıda insan isimleri
olarak anlatılan bu adreslere MAC (Ortam Erişim Yönetimi) adresi denir.
Bu adresler donanımsal adreslerdir ve internete bağlanabilen cihazlara üretici
tarafından entegre edilirler.

Buradan itibaren işin içine güvenlik endişeleri girmektedir çünkü herkese açık
bir ağda iletişim kurmak istediğiniz kişinin doğru kişi olup olmadığından emin
olamazsınız. Aynı sınıf örneğinde olduğu gibi herhangi bir kişi size gelip gerçekte
olduğu kişi konusunda yalan söyleyerek aslında başka bir kişiye gitmesi gereken
bilgiyi alabilir, değiştirebilir veya yanıtlayabilir. Bu durum ciddi bir güvenlik
tehlikesi oluşturmaktadır. Biraz da bu güvenlik tehlikesini doğuran protokolden
bahsedelim.

Adres çözümleme protokolü veya kısaca ARP, ağdaki IP adreslerini MAC
adreslerine eşleyen protokoldür. Bir cihazın IP adresi biliniyorsa ağa bir yayın
paketi gönderilerek IP’si bilinen cihazın MAC adresi istenir. Bu yayın paketine
ARP isteği, gelen cevaba da ARP cevabı denir. Bu, yukarıda bahsettiğimiz
sınıf örneğinin ağlardaki uygulamasıdır. ARP istekleri cihazlar arasında gidip
geldikten sonra ağdaki bütün cihazlar “ARP tablosu” adı verilen bir deftere
IP:MAC şeklinde bilgileri kaydederler. Ancak ARP, güvenli olmak üzere
tasarlanmamıştır. Şimdi de bu bilgiler ışığında ARP zehirlemesi ve ortadaki
adam saldırılarını anlayalım.

## Arp Zehirlemesi ve Ortadaki Adam Saldırısı
Saldırgan bu saldırıyı “ARP zehirlemesi” adlı teknik ile gerçekleştirir. ARP
zehirlemesi, saldırganların ağdaki cihazlar arasındaki iletişimi kesmesine olanak
sağlayan bir ortadaki adam saldırısıdır. Şimdi gelin bu saldırıya bir senaryo ile
bakalım.
1. Saldırgan kurban ile aynı ağa bağlanır ve bir ağ taraması yaparak modemin
ve kurban cihazın IP ve MAC adreslerini öğrenir.
2. Saldırgan sahte ARP yanıtları göndermek için birtakım araçlar kullanır.
3. Sahte yanıtlar modeme ve kurban cihaza ait iki IP adresi için de doğru
MAC adresinin, saldırganın MAC adresi olduğunu belirtir. Yani saldırgan,
kurbana modemin MAC adresi; modeme ise kurbanın MAC adresi
hakkında yalan söyleyip iki cihazın da ARP tablolarını güncellemelerine
sebep olarak ortadaki adam konumuna gelir. Bu andan itibaren kurbandan
çıkıp modeme giden bütün istekler ve modemden kurbana gelen bütün
yanıtlar saldırganın kontrolü altındadır.

## Çözüm: SSL
Şimdi sizin de tahmin etmiş olacağınız üzere, ortadaki adam konumuna gelen
saldırgan kurbanın bilgisayarından giden bütün istekleri dolayısıyla web sitelerine
girdiği kullanıcı adları, parolalar, kredi kartı bilgileri gibi bir çok hassas
bilgiyi ele geçirebilecek konumdadır. “Bu kadar kritik etkiye sahip bir sorunun
çözümü yok mu?” dediğinizi duyar gibiyiz. Gelin şimdi bu sorunun çözümüne
bakalım.

Bundan yaklaşık 26 yıl öncesine kadar yukarıda bahsettiğimiz sorunun bir
çözümü yoktu ve açık ağlarda internete çıkan hiçbir verinin güvenliği söz
konusu değildi. Ta ki 1995’te websiteleri SSL (Güvenli Soket Katmanı) 2.0
kullanmaya başlayana kadar.

SSL, websitesi ve kullanıcı arasında gidip gelen istek ve yanıtları şifrelediği için
güvenlidir. SSL şifreleme için iki adet anahtar bulunmaktadır. Bu anahtarlar,
dijital ortamda şifrelenmiş yazılımlardır. Bir anahtarın kilitlemiş olduğu veriyi,
sadece diğer anahtar açabilir. Bu anahtarlardan biri (private key) sunucuda
(websitesinde) kalır. Diğer anahtar (public key) ise, bağlantı kurmak istediğiniz
kişilere gönderilir.

Dışarıdan sizinle iletişime geçmek isteyen kişi, public key’i kullanarak mesajı
güvenli bir şekilde size gönderir. Veri, size ulaşmadan, transfer sırasında veriye
ulaşılsa bile, şifrenin çözülmesi için sunucuda (websitesinde) bulunan private
key gerekecektir.

Yani yukarıda bahsettiğimiz ortadaki adam saldırısına dönecek olursak, saldırgan
SSL kullanılan bir sistemde şifrelenmiş veriye ulaşsa da elinde private key
olmadığı için veriyi okuyamayacaktır.

## Açık Ağlarda Kendimizi Nasıl Koruruz?
Açık ağlarda ortadaki adam saldırısına maruz kalmamak için websitelerine
girdiğinizde tarayıcınızdaki adres çubuğunun solunda kilit işareti olup olmadığına
bakmalısınız. Kilit işareti bulunan websitelerinde SSL kullanılıyor
demektir.

Saldırganlar SSL’yi atlatmak için “sslstrip” adlı bir yöntem geliştirmişlerdir
ancak HSTS isimli bir mekanizma ile bu durum engellenebilmektedir ve
günümüzde güvenliğini önemseyen bütün websiteleri bu mekanizmayı uygulamaktadır.
Dolayısıyla çoğu durumda SSL bulunan web sitelerinde güvenli
olursunuz.

## KAYNAKÇA
- [Man-in-the-middle Saldırısı](https://tr.wikipedia.org/wiki/Man-in-the-middle_sald%C4%B1r%C4%B1s%C4%B1)
- [Man-in-the-middle Attacks](https://www.rapid7.com/fundamentals/man-in-the-middle-attacks/)
