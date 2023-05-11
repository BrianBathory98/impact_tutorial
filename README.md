# impact_tutorial

#install rustc

jika benar benar baru deploy vps maka ikutin langkah berikut: 

Pertama, mari kita pastikan mesin linux up-to-date.
```
sudo apt update && sudo apt upgrade -y
```
install dependencies (saya pakai ubuntu)
```
sudo apt install --assume-yes git clang curl libssl-dev protobuf-compiler
```
download rustup (pilih default saja)
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
update untuk include cargo
```
source $HOME/.cargo/env
```
pastikan sudah terinstal
```
rustc --version
```
pastikan tool stabil dan yg direkomendasikan
```
rustup default stable
rustup update
```
tambahin nightly dan wasm nya
```
rustup update nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```
verifikasi apa sudah terinstall dengan benar dulu
```
rustup show
rustup +nightly show
```
kalau output nya mirip mirip kek dibawah ini berarti good
```
# rustup show

active toolchain
----------------

stable-x86_64-unknown-linux-gnu (default)
rustc 1.62.1 (e092d0b6b 2022-07-16)

# rustup +nightly show

active toolchain
----------------

nightly-x86_64-unknown-linux-gnu (overridden by +toolchain on the command line)
rustc 1.65.0-nightly (34a6cae28 2022-08-09)
```

#lets go jalanin node nya
pertama clone dulu github impact
```
git clone https://github.com/GlobalBoost/impactprotocol
```
masuk impactprotocol
```
cd impactprotocol
```
realease build nya (butuh waktu yg lumayan,jgn close putty/mobaxterm mu,minimize saja)
```
cargo build --release
```
buat screen dulu khusus untuk node
```
screen -Rd impact
```
#jika distep ini disuruh masukin password, maka isi aja sesuka hati password nya asal ingat saja, disamaain semua saja

bikin wallet
```
./target/release/impact generate-mining-key --chain=impact-testnet
```
import wallet nya (isi phrase pakai " ")
```
./target/release/impact import-mining-key "secret phrasemu" \--base-path /tmp/impactnode2 \--chain=impact-testnet
```
terus input public key yg tadi di generate dan nama node yg ingin kalian buat ke dalam template ini
```
./target/release/impact \
--base-path /tmp/impactnode \
--chain=impact-testnet \
--port 30333 \
--ws-port 9945 \
--rpc-port 9933 \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
--validator \
--author public key disini \
--rpc-methods Unsafe \
--name namanodemu \
--password-interactive
```
jika sudah jalan maka detach dari screen agar tersimpan
menggunakan ctrl a + d


#jika sudah setup node maka bisa cek nama nodemu di
https://telemetry.polkadot.io/#list/0x1133ca761f24222cb0811f34641dba07acd88c77bd9f30a23a99c2cba233cb91

#setting nodemu untuk menjadi validator

siapkan dulu 2 wallet polkadotjs extension, 1 akun biasa, 1 lagi akun import an dari node mu
untuk stash validator
lalu klik link :
https://polkadot.js.org/apps/?rpc=wss://rpc.testnet.impactprotocol.network#/accounts
![image](https://github.com/BrianBathory98/impact_tutorial/assets/41656124/6c21bcb3-834f-4d34-a420-c33689b06581)


copy alamat wallet mu terus minta faucet di app signal (apk dihp / pc)
download dulu apk signal di hp/pc lalu klik link :
https://signal.group/#CjQKICa9F2r95FQoGhYjc02lwNgKZCOfDEZngfoWgr_ZkHc4EhAOywghKv4DebEkPsicSCFb

buat akun, lalu minta faucet untuk kedua wallet polkadotjs diatas

#membuat node menjadi validator
buka lagi vps mu lalu
```
curl -H "Content-Type: application/json" -d'{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[ ]}' http://localhost:9933
```
copy key nya dan simpan

lalu buka web https://polkadot.js.org/apps/?rpc=wss://rpc.testnet.impactprotocol.network#/accounts
buka bagian :
![image](https://github.com/BrianBathory98/impact_tutorial/assets/41656124/bb918177-ea6a-4c9f-9c9f-83461a32374d)

klik + di validator
![image](https://github.com/BrianBathory98/impact_tutorial/assets/41656124/4bc959cc-b925-41c1-bbdb-8062ca3a308e)

nah disini stash account = akun node
controller account = wallet biasa
![image](https://github.com/BrianBathory98/impact_tutorial/assets/41656124/a051fb9c-24b8-406d-b89b-1424a10d1a64)

lalu klik next (edit komisinya sesukamu)
![image](https://github.com/BrianBathory98/impact_tutorial/assets/41656124/355e1c08-2b04-4b94-9dbf-2b02fb98a129)

jika sudah maka validatormu akan menunggu untuk menjadi aktif
![image](https://github.com/BrianBathory98/impact_tutorial/assets/41656124/33f3d8eb-9eff-4916-aca0-0fed4ba04eb5)

#submit data nodemu

submit data node mu di github impact protocol :
https://github.com/GlobalBoost/incentivized-testnet/issues

done, jika mau bikin 2 node maka ulangi step dari buat screen, namakan screen impact 2
lalu masuk folder impactprotocol (cd impactprotocol) lanjutkan step selanjutnya seperti diatas

tapi edit bagian port seperti dibawah ini
```
./target/release/impact \
--base-path /tmp/impactnode2 \
--chain=impact-testnet \
--port 40333 \
--ws-port 7745 \
--rpc-port 7733 \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
--validator \
--author public_keymu \
--rpc-methods Unsafe \
--name nama_nodemu \
--password-interactive
```
dan untuk ambil rotatekeys nya dengan
```
curl -H "Content-Type: application/json" -d'{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[ ]}' http://localhost:7733
```

lalu bikin lagi validator lagi seperti step step di atas

........tamat.....















