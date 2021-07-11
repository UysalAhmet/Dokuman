# WSL VSFTPD KURULUMU
## Sistem Güncelleştirmelerinin Kontrol Edilmesi
Sistem güncellemelerini kontrol edip eksik güncelleme varsa yüklüyoruz.
```
sudo apt update && sudo apt -y upgrade
```
## VSFTPD Kurulumu
Güncelleme işlemi bittikten sonra `vsftpd` programımızı kuruyoruz.
```
sudo apt install vsftpd -y
```
Yükleme işleminden sonra `vsftpd` programımızı başlatıp etkinleştiriyoruz.
```
sudo service vsftpd start
sudo systemctl enable vsftpd
```
Programımızın çalışıp çalışmadığını kontrol ediyoruz
```
sudo service vsftpd status
```
## FTP Kullanıcısı Oluşturma
Kullanıcımızı oluştururken `ftpuser` yazan kısıma kendi oluşturmak istediğiniz kullanıcı adını verebilirsiniz.
```
sudo useradd -m ftpuser
```
Kullanıcımızın şifresini belirliyoruz.
```
sudo passwd ftpuser
```
## Kullanıcımız İçin Dizin Oluşturma Ve Yetkilendirme
Home altında kullanıcımız için dizinlerimizi oluşturuyoruz. 
```
sudo mkdir /home/ftpuser/ftp
sudo chown nobody:nogroup /home/ftpuser/ftp/
sudo chmod a-w /home/ftpuser/ftp/
ll /home/ftpuser/
```
Download ve Upload işlemleri için kullanılacak bir dizin oluşturuyoruz.
```
sudo mkdir /home/ftpuser/ftp/data
sudo chown  ftpuser:ftpuser /home/ftpuser/ftp/data/
ll /home/ftpuser/ftp
```
## VSFTPD Yapılandırması
Yapılandırma dosyamızın bozulması durumunda sıkıntı yaşamamak için yedeğini alıyoruz.
```
sudo mv /etc/vsftpd.conf /etc/vsftpd.conf.bak
```
Düzenleme yapmak için `vsftpd.conf` dosyasını açıyoruz.
```
sudo nano /etc/vsftpd.conf
```
İçerisine aşağıda verdiğim kodları yapıştırıyoruz.
```
# vsFTP Server on Ubuntu - Part 2 - Allow Users Local Access FTP Server - Restrict Home Directory
#
#Run standalone
#
listen=NO
listen_ipv6=YES

#
#Not allow anonymous FTP
#
anonymous_enable=NO

#
#Controls whether local logins are permitted or not. 
#If enabled, normal user accounts in /etc/passwd (or wherever your PAM config references) may be used to log in. 
#This must be enable for any non-anonymous login to work, including virtual users.
#
local_enable=YES

#
# Allow users with local accounts to upload files via FTP
#
write_enable=YES

#
#The value that the umask for file creation is set to for local users. 
#
local_umask=022

#
#If enabled, users of the FTP server can be shown messages when they first enter a new directory.
#
dirmessage_enable=YES

#
#If enabled, vsftpd will display directory listings with the time in your local time zone. The default is to display GMT. 
#The times returned by the MDTM FTP command are also affected by this option.
#
use_localtime=YES

#
#If enabled, a log file will be maintained detailling uploads and downloads. 
#By default, this file will be placed at /var/log/vsftpd.log, but this location may be overridden using the configuration setting vsftpd_log_file
#
xferlog_enable=YES

#
#This controls whether PORT style data connections use port 20 (ftp-data) on the server machine. 
#For security reasons, some clients may insist that this is the case. 
#Conversely, disabling this option enables vsftpd to run with slightly less privilege
#
connect_from_port_20=YES

#
#If set to YES, local users will be (by default) placed in a chroot() jail in their home directory after login.
#
chroot_local_user=YES

#
#This string is the name of the PAM service vsftpd will use.
#
pam_service_name=vsftpd

#
#If activated, files and directories starting with . will be shown in directory listings even if the "a" flag was not used by the client. 
#This override excludes the "." and ".." entries.
#
force_dot_files=YES

#
# Limit the range of ports that can be used for passive FTP
#
pasv_min_port=40000
pasv_max_port=50000

#
#This option is useful is conjunction with virtual users. It is used to automatically generate a home directory for each virtual user.
#
user_sub_token=$USER

#
#This option represents a directory which vsftpd will try to change into after a local (i.e. non-anonymous) login. Failure is silently ignored.
#
local_root=/home/$USER/ftp

#
#If enabled, vsftpd will load a list of usernames, from the filename given by userlist_file. 
#If a user tries to log in using a name in this file, they will be denied before they are asked for a password. 
#This may be useful in preventing cleartext passwords being transmitted. See also userlist_deny.
#
userlist_enable=YES

#
#This option is the name of the file loaded when the userlist_enable option is active.
#
userlist_file=/etc/vsftpd-users

#
#This option is examined if userlist_enable is activated. 
#If you set this setting to NO, then users will be denied login unless they are explicitly listed in the file specified by userlist_file. 
#When login is denied, the denial is issued before the user is asked for a password.
#
userlist_deny=NO

```
Yaptığımız değişikliği kaydetmek için sırasıyla `Ctrl`+`X` tuşlarına basıyoruz.Ardından bize değişiklikleri kaydetmek isteyip istemediğimizi soruyor `Y` tuşuna basıyoruz ve klavyemizden `Enter` tuşuna basarak onaylıyoruz. Yukarıdaki dosyada  kullanıcı listemizi `userlist_file=/etc/vsftpd-users` kodu ile `vsftpd-users` olarak belirttik. Böyle bir dosya şuan bulunmadığı için oluşturup içerisine kullanıcımızı ekliyoruz.
```
sudo nano /etc/vsftpd-users
```
Açılan dosyaya kullanıcı adımızı ekliyoruz ( Bu örnekte `ftpuser`) ve aynı şekilde kaydediyoruz. Vsftpd programını yeniden başlatıyoruz.
```
sudo service vsftpd restart
```
## Test
`FileZilla` Programını kullanarak kurulumda herhangi bir problem olup olmadığını anlayabiliriz. Programı açtığımızda bizden `Sunucu` kısmında FTP serverin IP sini istiyor bu IP yi bulmak için WSL terminalinden `ifconfig` komutuyla bulabilirsiniz bu komutun çalışması için `net-tools` programının kurulu olması gerekiyor kurmak için aşağıdaki komutu kullanabilirsiniz.
```
sudo apt install net-tools
```
Kurulum bittikten sonra `ifconfig` komutunu girip IP mizi öğrenebiliriz. `eth0:` altındaki `inet` in yanındaki IP bizim IP mizdir. Bilgisayarınız her yeniden başlatıldığında bu IP değişir FTP servere bağlanmak için her açılışta tekrar kontrol etmek gerekir.

Sunucumuzu girdikten sonra `Kullanıcı adı` olarak oluşturduğumuz kullanıcı adını `Parola` olarakda oluşturduğumuz parolayı girip `Hızlı bağlan` butonuna basın eğer bir sorun yok ise bağlantınız başarılı  bir şekilde yapılacaktır. Sol panelde bulunan `Yerel Site` kısmından dosyaları `Uzak Site` ye yükleyebilirsiniz WSL üzerindende dosyaların başarılı bir şekilde aktarıldığını kontrol edebilirsiniz.

