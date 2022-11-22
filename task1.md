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

