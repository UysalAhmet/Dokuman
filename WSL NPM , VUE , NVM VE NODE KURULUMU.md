# WSL NPM , VUE , NVM VE NODE KURULUMU
# CURL Kurulumu
İnternetten indirme yaparken kullanılan bir araç olan `curl` kurulumu için aşağıdaki komutu kullanabiliriz.
```
sudo apt-get install curl
```
## NVM Kurulumu
NVM kurmak için aşağıdaki kodu kullanabiliriz.
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```
Eğer bu kullanım çalışmıyorsa link üzerinde bir güncelleme olup olmadığını https://github.com/nvm-sh/nvm adresinden kontrol edebilirsiniz. Kurulum tamamlandıktan sonra terminali kapatıp açmamız gerekiyor ardından aşağıdaki komut ile `nvm` yüklemelerimiz listelenir.
```
nvm ls
```
Komutun çıktısı aşağıdaki gibi olmalıdır.
```
            N/A
iojs -> N/A (default)
node -> stable (-> N/A) (default)
unstable -> N/A (default)
```
## NODE ve NPM Kurulumu
En güncel sürümünü yüklemek içinğıdaki kodu kullanabiliriz.
```
nvm install --lts
```
## VUE Kurulumu
NPM üzerinden kurulum yapmak için aşağıdaki komutu kullanabilirsiniz.
```
npm install -g @vue/cli
```
Kullandığımız Vue nin versiyonunu öğrenmek için aşağıdaki komutu kullanabilirsiniz.
```
vue --version
```

## VUE Test
Yeni bir vue projesi oluşturmak için aşağıdaki komutu girebilirsiniz.
```
vue create vue-proje-adi
```
Kurulum sırasında sizden bazı bilgiler isteyebilir onları doldurduktan sonra kuruluma devam edebilirsiniz. Oluşturduğumuz projenin dizinine girip projeyi build ve run etmek için aşağıdaki komutları kullanabilirsiniz.
```
npm run build // projeyi derler
npm run serve //localhost üzerinden yayına açar
```













