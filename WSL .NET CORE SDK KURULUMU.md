# WSL .NET CORE SDK KURULUMU
## Ubuntu 20.04 için .NET Core 2.1 SDK ve Runtime Kurulumu
```
wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb
```
Yukarıdaki komutlar ile paketleri yüklüyoruz. Ardından aşağıdaki komutlar ile `dotnet-sdk-2.1` kurulumunu yapıyoruz
```
sudo apt-get update; \
  sudo apt-get install -y apt-transport-https && \
  sudo apt-get update && \
  sudo apt-get install -y dotnet-sdk-2.1
```
Yüklediğiniz SDK nin versiyonunu aşağıdaki komut ile öğrenebilirsiniz.
```
dotnet --version
```
## SDK Testi
Test uygulamasını oluşturmak istediğiniz dizine girip aşağıdaki komutları kullanarak bir `console` uygulaması oluşturabilirsiniz.
```
dotnet new console
```
Oluşturduğunuz proje basit bir `console` uygulaması olup ekrana çıktı olarak 'Hello World!' yazdırmaktadır. Build ve Run etmek için aşağıdaki komutları kullanabilirsiniz.
```
dotnet build
dotnet run
```
Birden fazla projenin olduğu bir dizindeyken çalıştırmak istediğimiz projeyi seçmek için aşağıdaki yöntem kullanılır.
```
dotnet build --project Proje.Proje1
dotnet run --project Proje.Proje1
```
Buradaki `Proje` ve `Proje1` sizin dizininizdeki projelerin konumlarıdır.



