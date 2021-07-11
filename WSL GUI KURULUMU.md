# WSL GUI Kurulumu
## Sistem Güncelleştirmelerinin Kontrol Edilmesi
```
sudo apt update && sudo apt -y upgrade
```
Sistem güncellemelerini kontrol edip eksik güncelleme varsa yüklüyoruz.
## XRDP Kurulumu
Öncelikle sistemimizde daha öncesinde `XRDP` kurulmuş ise onu temizliyoruz.
```
sudo apt-get purge xrdp
```
Kurulum için aşağıdaki komutu kullanıyoruz. Bu biraz uzun sürebilir.
```
sudo apt install -y xrdp
```
## XFCE4 Kurulumu
Xfce4, GNU/Linux ve Unix benzeri sistemlerle uyumlu bir masaüstü ortamıdır.
Düşük donanım özelliklerine sahip makineler için idealdir. Bizde WSL üzerinde
işlem yaptığımız için bizim için idealdir.
```
sudo apt install -y xfce4
```
İndirme işlemi tamamlandığında önümüze bir ekran gelir ve bizden `gdm3` veya `lightgdm` seçeneklerinden birini seçmemizi ister. Varsayılan olarak `gdm3` seçili gelir. Klayveden `Enter` tuşuna basarak onaylayalım. Kurulum tamamlandıktan sonra içerisinde kullanımı kolaylaştıracak eklentiler bulunan `xfce4-goodies` paketinide yüklüyoruz.
 ```
sudo apt install -y xfce4-goodies
 ```
 ## XRDP Ayarları
 Aşağıdaki komutları sırasıyla uygulayarak `Xfce4` ve `Xrdp` yapılandırmamızı tamamlıyoruz.
 ```
 sudo cp /etc/xrdp/xrdp.ini /etc/xrdp/xrdp.ini.bak
sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini
sudo sed -i 's/max_bpp=32/#max_bpp=32\nmax_bpp=128/g' /etc/xrdp/xrdp.ini
sudo sed -i 's/xserverbpp=24/#xserverbpp=24\nxserverbpp=128/g' /etc/xrdp/xrdp.ini
echo xfce4-session > ~/.xsession
 ```
 `sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini` Burada  hangi port üzerinden bağlanacağımızı belirtmiş olduk.
```
sudo nano /etc/xrdp/startwm.sh
```
Terminal üzerinden açmış olduğumuz `startvm.sh` dosyasını açıyoruz. 
```
test -x /etc/X11/Xsession && exec /etc/X11/Xsession
exec /bin/sh /etc/X11/Xsession
```
Açmış olduğumuz dosyanın en alt kısmında yukarıda görmüş olduğunuz iki satır bulunmaktadır bu iki satırı başına `#` işareti koyarak yorum satırı haline getiriyoruz ve hemen altına aşağıda görmüş olduğunuz satırı ekliyoruz
```
# xfce
startxfce4
```
Yaptığımız değişikliği kaydetmek için sırasıyla `Ctrl`+`X` tuşlarına basıyoruz.Ardından bize değişiklikleri kaydetmek isteyip istemediğimizi soruyor `Y` tuşuna basıyoruz ve klavyemizden `Enter` tuşuna basarak onaylıyoruz. Yaptığımız değişikliklerin geçerli olabilmesi için `Xrdp` yeniden başlatılmalı.
```
sudo /etc/init.d/xrdp start
```
Xrdp yeniden başlatıldığına göre kurulum tamamlanmış demektir.
## Uzak Masaüstü Bağlantısı
Bilgisayarın arama kısmına `Uzak Masaüstü Bağlantısı` yazdığımızda karşımıza gelen uygulamayı açıyoruz. Uygulama bizden bağlanılacak uzak masaüstünün adresini isteyecek biz WSL üzerinden local bilgisayarımızda işlem yaptığımız için `localhost` bilgisayarına bağlanacağız port olarak da yukarıda belirttiğimiz `3389` veya `3390` portunu kullanacağız. Bilgisayar kısmına `localhost:3390` yazıyoruz ve `Bağlan` diyoruz. `Uzak bilgisayarın kimliği doğrulanamıyor. Yinede bağlanmak istiyor musunuz ?` Uyarısına `Evet` diyoruz. Karşımıza bir giriş ekranı geliyor `username` kısmına WSL üzerinde kullandığınız kullanıcı adını ve `password` kısmına yine aynı şekilde WSL üzerindeki kullanıcınızın parolasını yazıyorsunuz. Eğer bu kısımda bir hata alıyorsanız muhtemelen kullanıcı adınızı veya parolanızı yanlış giriyorsunuzdur. Ayrıca belirtmek isterimki oturum açma ekranındaki klayve düzeni ingilizcedir.
## Klayve Düzeninin Değiştirilmesi
Oturum açma ekranındaki klayve düzeninin nasıl değiştirileceğini henuz bulamadım ama giriş yaptıktan sonra varsayılan olarak türkçe klavyeye geçmek için sağ üstte bulunan `Applications` sekmesi altında `Settings` altındaki `Keyboard` a tıklıyoruz. Önümüze açılan pencerede en alt kısımda bulunan `Test area` da klavyenizdeki değişikliği test edebilirsiniz. Buradan `Layout` sekmesine geliyoruz `Use System defaults` seçimini kaldırıyoruz.`Keyboard Layout` altından klavyenizi değiştirebilirsiniz. Türkçe yapmak için varsayılan olarak  gelen klavyeyi seçip altında bulunan `Edit` butonuna basıyoruz. Önümüze gelen diller arasından `Turkish` i seçiyoruz.
