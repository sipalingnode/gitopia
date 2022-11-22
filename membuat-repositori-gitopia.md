# Gitopia membuat Repository

1. Login ke https://gitopia.com/
2. Connect Via Recovery Exisiting Wallet
3. Masukan Pharse Wallet Kepler/Validator Kalian
4. Buat Password dan Done
5. Create Repository, Buat Nama dan Deskripsi Bebas

## Open VPS

## Install Git Remote 
```curl https://get.gitopia.com | bash```
## Buat Folder (sesuaikan yang ada di web)
```
mkdir nama-repository
``` 
**nama repository yang diweb**
```
cd nama-repository
```
**nama repository yang baru di buat tadi**

## Buat Config
```
git config --global user.email "email lu bang"
```
```
git config --global user.name "Akun-Gitopia"
```

## Buat File readme.md (langsung salin aja kgk usah di edit biar gampang)
```
echo "# welcome to ASC" >> README.md
git init
git add README.md
git commit -m "initial commit"
```

## Push Repository

- Login ke https://gitopia.com/
- Klik Profil & Download Wallet
<p align="center">
  <img height="500" height="auto" src="https://user-images.githubusercontent.com/109174478/203283259-e71d9c99-06bc-45d4-8858-cd2d3dd07d56.jpg">
</p>

- Upload wallet yang udah di download ke VPS kalian.
<p align="center">
  <img height="500" height="auto" src="https://user-images.githubusercontent.com/109174478/203286040-9ab814e1-b2f2-4589-aff6-4b932ed6adb1.jpg">
</p>

## Export Wallet
```
export GITOPIA_WALLET=/root/armytestnet/armytestnet.json
```
**armytestnet ganti pake nama folder atau akun kalian**
## Upload File ke Akun Gitopia
```
git remote add origin gitopia://armytestnet/Testnet
git push -u origin master
```
**armytestnet ganti pake nama akun lu yang di web**
<p align="center">
  <img height="300" height="auto" src="https://user-images.githubusercontent.com/109174478/203288680-5d4df04a-9224-42ad-931a-725489796a28.jpg">
</p>

## Done. Berhasil
<p align="center">
  <img height="500" height="auto" src="https://user-images.githubusercontent.com/109174478/203289641-8a88548d-a5ac-4136-a4b0-352e97e8401b.jpg">
</p>
