# WSL MYSQL KURULUMU
## Önceki Yüklemelerin Temizlenmesi
Eğer sistemimizde önceden yüklü `mysql` yüklemesi varsa onları temizleyerek temiz kuruluma başlayalım.
```
sudo apt-get remove --purge *mysql*
sudo rm -rf /etc/mysql /var/lib/mysql
sudo apt-get remove --purge *mariadb*
sudo apt-get autoremove
sudo apt-get autoclean
```
Yüklemelerin kaldırıldığını doğrulamak için aşağıdaki komutları kullanabilirsiniz.
```
dpkg -l | grep mariadb
dpkg -l | grep mysql
```
## Sistem Güncelleştirmelerinin Kontrol Edilmesi
Sistem güncellemelerini kontrol edip eksik güncelleme varsa yüklüyoruz.
```
sudo apt-get install -f
sudo apt update && sudo apt -y upgrade
```
## MYSQL Kurulumu
Aşağıdaki komut ile `mysql` programını kuruyoruz.
```
sudo apt install mysql-server
```
Yükleme bittikten sonra `mysql` programını başlatıyoruz.
```
sudo service mysql start
```

## MYSQL Güvenlik Ayarlamaları
Aşağıdaki kod ile güvenlik yapılandırmasına başlıyoruz
```
sudo mysql_secure_installation
```
Komut çalıştıktan sonra aşağıdaki soru karşımıza geliyor.
```
VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?
Press y|Y for Yes, any other key for No:
```
Kısaca güvenli şifreleme kullanmak isteyip istemediğimizi soruyor evet için `y` hayır için `n` tuşuna basmamız lazım. Güvenli şifreleme istemediğimiz durumdan devam edersek bizden parolamızı girmemizi isteyecek parolamızı belirleyip devam ediyoruz.
```
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.
Remove anonymous users? (Press y|Y for Yes, any other key for No)
```
Kısacası anonim kullanıcıların silinip silinmemesi için kara vermemiz gerekiyor. `y` Basıp devam ediyoruz. Sıradaki soru
```
Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.
Disallow root login remotely?
```
Kısaca root girişi için dışardan erişim olsun mu diye soruyor `y` basıp devam ediyoruz. Sıradaki soru
```
Remove test database and access to it? (Press y|Y for Yes, any other key for No)
```
Test veritabanı kaldırılsın mı diye soruyor `y` basıp devam ediyoruz. Sıradaki soru
```
Reload privilege tables now?
```
Değişiklikler yeniden yüklensin mi diye soruyor `y` basıp devam ediyoruz ve güvenlik yapılandırmamız bitiyor.

## MYSQL Veritabanına Bağlanma
```
mysql -u root  -p
```

Yukarıdaki komutu terminale yazdığınız ve bilgilerinizi doğru bir şekilde girdiğiniz halde `` hatası alıyorsanız uygulayabileceğimiz iki tane yöntem var.
### 1.Yöntem Root İçin Kimlik Doğrulama Yönteminin Değiştirilmesi
```
sudo mysql
```
Yukarıdaki komut ile veritabanımıza bağlanıyoruz ve aşağıdaki komut yardımıyla kullanıcılarımızın kimlik doğrulama yöntemlerini görüyoruz.
```
SELECT user,authentication_string,plugin,host FROM mysql.user;
```
Gördüğünüz gibi `root` kullanıcısının `plugin` özelliği ` auth_socket` bunu değiştirmek için aşağıdaki komutu kullanıyoruz `password` yazan yere kendi kullanmak istediğiniz şifreyi yazabilirsiniz.
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
Bu komutu çalıştırdıktan sonra değişikliklerin geçerli olabilmesi için aşağıdaki komutu kullanıyoruz.
```
FLUSH PRIVILEGES;
```
 Tekrardan kullanıcıları listelersek `auth_socket` olan özelliğin `mysql_native_password` olarak değiştiğini görebilirsiniz. Mysql komut stırına `exit` yazıp Mysql den çıkabilirsiniz. Tekrar giriş yapmak için aşağıdaki komutu kullanabilirsiniz. Buradaki `-u` dan sonra giriş yapmak istediğimiz kullanıcının adını giriyoruz `-p` ise parola ile giriş yapacağımızı belirtiyor.
 ```
 mysql -u root -p
 ```
 Yukarıda belirlediğiniz parola ile giriş yaparken artık bir sorun yaşamamanız gerekiyor.
### 2.Yöntem Yeni Bir Kullanıcı Oluşturulması
 ```
sudo mysql
```
Yukarıdaki komut ile veritabanımıza bağlanıyoruz ve aşağıdaki komut yardımıyla kullanıcılarımızın kimlik doğrulama yöntemlerini görüyoruz.
```
SELECT user,authentication_string,plugin,host FROM mysql.user;
```
Yeni bir kullanıcı oluşturmak için aşağıdaki komutu kullanabiliriz buradaki `newuser` yerine kendi belirlediğimiz kullanıcı adını, `password` yerinede kendi belirlediğimiz parolayı girebiliriz. Eğer `localhost` yerine `%` işareti koyarsak kullanıcımızın dışarıdan bir yerden de bağlanabileceğini belirtmiş oluruz.
```
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
```
Yeni oluşturduğumuz kullanıcının veritabanı üzerinde henüz hiç bir yetkisi yoktur. Aşağıdaki komut ile yetkilendirme işlemini yapabiliriz.
```
 GRANT ALL PRIVILEGES ON * . * TO 'newuser'@'localhost';
``` 
Değişikliklerin kaydedilmesi için aşağıdaki kodu yazıyoruz.
```
 FLUSH PRIVILEGES;
```
Tekrardan kullanıcıları listeleyip oluşturduğumuz kullanıcıyı görebiliriz.
```
SELECT user,authentication_string,plugin,host FROM mysql.user;
```
Mysql komut stırına `exit` yazıp Mysql den çıkabilirsiniz. Tekrar giriş yapmak için aşağıdaki komutu kullanabilirsiniz. Buradaki `-u` dan sonra giriş yapmak istediğimiz kullanıcının adını giriyoruz `-p` ise parola ile giriş yapacağımızı belirtiyor.
 ```
 mysql -u newuser -p
 ```
 Yukarıda belirlediğiniz parola ile giriş yaparken artık bir sorun yaşamamanız gerekiyor.
## Örnek Bir Veritabanı Yedeğinin Yüklenmesi
Öncelikle `.sql` uzantılı dosyanın bulunduğu konuma geliyoruz.Eğer veriler ve tablolar ayrı ayrı oluşturulmuş ise önce veritabanının ve tabloların olusturulması gerekir. `Sakila` veritabanı için `.sql` uzantılı dosyaların bulundugu konumda iken terminalden veritabanına giriş yapıyoruz. Giriş yaptıktan sonra aşağıdaki komut ile önce veritabanını ve tabloları oluşturuyoruz.
```
source sakila-schema.sql
```
Ardından veritabanına verileri yüklüyoruz. Bunun için önce veritabanına erişim sağlamamız gerekir.
```
use sakila
source sakila-data.sql;
```





