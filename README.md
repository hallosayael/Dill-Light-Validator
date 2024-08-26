# Dill Light Validator Incentive Testnet (Public)

![dillxyz](https://github.com/user-attachments/assets/0c34bde0-8717-40e4-a970-3847e762092a)

Twitter: https://x.com/dill_xyz_

Discord: https://discord.com/invite/dill

Explorer: https://andes.dill.xyz/

Tutorial Official Docs: https://past-chokeberry-e85.notion.site/andes-doc-a23a9f71a85a4be9a6737e54d2674060

## Spesifikasi Hardware Minimum

2 Cores, 2G Ram,  min 40G SSD, Ubuntu 22.04

## 1. Persiapan Server
```
sudo apt update && apt upgrade -y
sudo apt install wget curl make clang net-tools pkg-config libssl-dev build-essential jq lz4 gcc unzip snapd -y
```
## 2. Install Node

### 2.1 Download & Extract package
```
cd $HOME && curl -O https://dill-release.s3.ap-southeast-1.amazonaws.com/linux/dill.tar.gz && \
tar -xzvf dill.tar.gz && cd dill
```
### 2.2 Generate Validator Keys
```
./dill_validators_gen new-mnemonic --num_validators=1 --chain=andes --folder=./
```

`contoh output`

```
ubuntu@ip-xxxx:~/dill$ ./dill_validators_gen new-mnemonic --num_validators=1 --chain=andes --folder=./

***Using the tool on an offline and secure device is highly recommended to keep your mnemonic safe.***

Please choose your language ['1. العربية', '2. ελληνικά', '3. English', '4. Français', '5. Bahasa melayu', '6. Italiano', '7. 日本語', '8. 한국어', '9. Português do Brasil', '10. român', '11. Türkçe', '12. 简体中文']:  [English]: 3
Please choose the language of the mnemonic word list ['1. 简体中文', '2. 繁體中文', '3. čeština', '4. English', '5. Italiano', '6. 한국어', '7. Português', '8. Español']:  [english]: 4
Create a password that secures your validator keystore(s). You will need to re-enter this to decrypt them when you setup your Dill validators.:
Repeat your keystore password for confirmation:
The amount of DILL token to be deposited(2500 by default). [2500]:
This is your mnemonic (seed phrase). Write it down and store it safely. It is the ONLY way to retrieve your deposit.


Creating your keys.
Creating your keystores:	  [####################################]  1/1
Verifying your keystores:	  [####################################]  1/1
Verifying your deposits:	  [####################################]  1/1

Success!
Your keys can be found at: ./validator_keys


Press any key.
ubuntu@ip-xxxx:~/dill$
```
Ini akan menghasilkan kunci validator dan menyimpannya di  ./validator_keys directory 

```
ls -ltr ./validator_keys
```
### 2.3 Impor keys Anda ke penyimpanan keys Anda
```
./dill-node accounts import --andes --wallet-dir ./keystore --keys-dir validator_keys/ --accept-terms-of-use
```
### 2.4 Tulis kata sandi yang Anda konfigurasikan pada langkah sebelumnya ke dalam sebuah file:
```
echo dillnode@123 > walletPw.txt
```
`dillnode@123` adalah kata sandi Anda, Anda dapat mengubahnya.

### 2.5 Menjalankan node light validator
```
./start_light.sh -p walletPw.txt
```
`contoh keluaran`

```
ubuntu@xxxxx:~/dill$ ./start_light.sh -p walletPw.txt
Option --pwdfile, argument 'walletPw.txt'
Remaining arguments:
using password file at walletPw.txt
start light node
start light node done
ubuntu@xxxxx:~/dill$ nohup: redirecting stderr to stdout
```
### 2.6 Cek kesehatan node
```
./health_check.sh -v
```
![dillxyz4](https://github.com/user-attachments/assets/52690e7d-51f1-4b30-be8f-5b9a568dfd64)


## 3. Staking

### 3.1 Faucet

Dapatkan faucet ke dompet EVM Anda dari channel Andes. Dompet yang Anda gunakan sebelumnya di galxe.

Gunakan MobaXterm atau Termius, Lalu masuk ke folder direktori `/dill/validator_keys` dan dapatkan file `deposit_data-xxxx.json` dan mengunggahnya ke situs di bawah ini.

### 3.2 Kunjungi https://staking.dill.xyz/

Upload file deposit_data-*.json

![dillxyz1](https://github.com/user-attachments/assets/01ecd1a4-0fa3-4b4c-ad4f-1e638f9b643c)

Connect ke MetaMask，pastikan Anda memiliki saldo DILL（>2500 DILL）

![dillxyz2](https://github.com/user-attachments/assets/9c393a9a-a9a8-43fb-b867-afdfb03aaffa)

Send deposit，gunakan MetaMask untuk mengirim transaksi deposit

![dillxyz3](https://github.com/user-attachments/assets/7f4008e0-35d3-4484-b219-e149d3d4059b)

## 4. Services Management
```
cd $HOME/dill
```

### 4.1 Cek kesehatan node
```
./health_check.sh -v
```
### 4.2 Menstop node

```
ps -ef | grep dill-node | grep -v grep | awk '{print $2}' | xargs kill
```
### 4.3 Menjalankan node

```
./start_light.sh -p walletPw.txt
```
### 4.4 Cek logs

```
tail -f -n 100 $HOME/dill/light_node/logs/dill.log
```

### 4.5 Cek syncing
Pastikan kolom sync_distance:0 dan is_syncing:false
```
curl -s localhost:3500/eth/v1/node/syncing
```

### 4.6 Cek di Explorer

Copy pubkey Anda di deposit_data-xxxx.json dan cari di explorer: https://andes.dill.xyz/validators
