# SecureCRT on Ubuntu 22.04 Jammy Jellyfish (Desktop)

As of August 2022 there isn't an official version for 22.04.  The last version is for 20.04.  To get that working on 22.04, a few dependencies are required.

##  libssl1.1 from Ubuntu Focal (20.04) updates pool:
```
wget http://ge.archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2.16_amd64.deb
sudo dpkg -i libssl1.1_1.1.1f-1ubuntu2.16_amd64.deb
```

## libicu66 from Impish (21.10) pool (may disappear in the future):
```
wget http://archive.ubuntu.com/ubuntu/pool/main/i/icu/libicu66_66.1-2ubuntu2_amd64.deb
sudo dpkg -i libicu66_66.1-2ubuntu2_amd64.deb
```
