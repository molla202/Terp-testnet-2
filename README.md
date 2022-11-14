# Terp-testnet-2

## kurulum script
```
source <(curl -s https://raw.githubusercontent.com/nodejumper-org/cosmos-scripts/master/terp/athena-2/install.sh)
```

## upgrade
```
sudo systemctl stop terpd

cd || return
rm -rf terp-core
git clone https://github.com/terpnetwork/terp-core.git
cd terp-core || return
git checkout v0.1.2
make install
terpd version # v0.1.2

terpd config chain-id athena-2
terpd tendermint unsafe-reset-all --home $HOME/.terp

curl -s https://raw.githubusercontent.com/terpnetwork/test-net/master/athena-2/genesis.json > $HOME/.terp/config/genesis.json
sha256sum $HOME/.terp/config/genesis.json # b2acc7ba63b05f5653578b05fc5322920635b35a19691dbafd41ef6374b1bc9a

seeds=""
peers="15f5bc75be9746fd1f712ca046502cae8a0f6ce7@terp-testnet.nodejumper.io:26656,7e5c0b9384a1b9636f1c670d5dc91ba4721ab1ca@23.88.53.28:36656,14ca69edabb36c51504f1a760292f8e6b9190bd7@65.21.138.123:28656,c989593c89b511318aa6a0c0d361a7a7f4271f28@65.108.124.172:26656,08a0f07da691a2d18d26e35eaa22ec784d1440cd@194.163.164.52:56656"
sed -i -e 's|^seeds *=.*|seeds = "'$seeds'"|; s|^persistent_peers *=.*|persistent_peers = "'$peers'"|' $HOME/.terp/config/config.toml

sudo systemctl restart terpd
sudo journalctl -u terpd -f --no-hostname -o cat
```



## snap
```
# install dependencies, if needed
sudo apt update
sudo apt install lz4 -y
```
```
sudo systemctl stop terpd

cp $HOME/.terp/data/priv_validator_state.json $HOME/.terp/priv_validator_state.json.backup
terpd tendermint unsafe-reset-all --home $HOME/.terp --keep-addr-book

rm -rf $HOME/.terp/data 
rm -rf $HOME/.terp/wasm

SNAP_NAME=$(curl -s https://snapshots2-testnet.nodejumper.io/terpnetwork-testnet/ | egrep -o ">athena-2.*\.tar.lz4" | tr -d ">")
curl https://snapshots2-testnet.nodejumper.io/terpnetwork-testnet/${SNAP_NAME} | lz4 -dc - | tar -xf - -C $HOME/.terp

mv $HOME/.terp/priv_validator_state.json.backup $HOME/.terp/data/priv_validator_state.json

sudo systemctl restart terpd
sudo journalctl -u terpd -f --no-hostname -o cat
```

## eşleşmesse peer
```
peers="15f5bc75be9746fd1f712ca046502cae8a0f6ce7@terp-testnet.nodejumper.io:30656,012dbc19c31c99c8a6a074868d5b6e9f57f8e100@67.205.150.113:26656,14ca69edabb36c51504f1a760292f8e6b9190bd7@65.21.138.123:28656,a7ecdd13c4e952d39024b75888911d00beb1c971@45.87.104.49:28656,1f5b5de284d47acd69ba73461fb6894a051bec59@51.79.28.170:26656,7dbcb53d5597289a9dd3ec28caf828a72edb75fb@89.179.33.100:33656,00c5107990c8f62c90bbbb3e55063b564d016eac@147.182.155.226:33656,88497ab3bbbcc1e8545771f45020e738bcce590f@46.138.245.164:26465,47d1764f523d4ec245ace891f69d59912c765f4e@209.126.81.240:26649,11e80b3f98422316e5b17b6d868bb346c36575ce@147.182.159.24:33656,85ead5036c471f8fe5474abf3d5eac324773d616@147.182.151.4:33656,4bb9ae0151e5d117847d8fcff9ef736fc17fff1a@137.184.164.31:33656,c88a36db47a5f8dded9cd1eb5a7b1af75e5d9294@217.13.223.167:60656,08a0f07da691a2d18d26e35eaa22ec784d1440cd@194.163.164.52:56656,61a00940fde08cc55824e48bdfb92ba938ba9c25@135.181.138.161:18656,8182dbd393cc2287b7684cb27f7fd7b2d29a94f1@143.198.236.149:33656,c989593c89b511318aa6a0c0d361a7a7f4271f28@65.108.124.172:26656,52d58d90c9456f4bf0a2c9c871d82ab49ee15992@135.181.35.46:26656,2c1ede9a5938e2b2f6900538eebf0c30f8925f72@65.21.193.112:26546,19566196191ca68c3688c14a73e47125bdebe352@62.171.171.91:26656,fff79c75baf152f39232f0abc493c0ccf0f2f5c1@161.97.77.219:33656,3122336186c16b9ba7f309afbac06412183121f8@65.108.103.86:56656,c1e0d990b831ba43d9d5fc65ebace627e25836bb@65.108.235.107:33656,d3d318d602d19d49e2a0009c74456e76a89d8173@147.182.151.231:33656,c73dc07274fa184ceb9dbe35aa4cd75e75f3a6e8@95.217.207.236:18656,bddd8d5ab48db625e6b235808c0bfa2fa6a697fe@65.21.199.148:26620,2f0f98eb3965cc9949073b1f0e75a5e55be44ed2@65.109.28.177:21856,a92fdea6039f1edd8ef60e85c6a0a624e9fb669b@65.108.43.116:56155,838705dcbf225c5340156c275a35e9c2c7985186@65.108.99.250:10656,dd7ce08ca73b46172141894ab535b84af8152c56@38.242.202.200:26656,d2af3d86ee5698037d802567ed930f8d58d89c25@38.242.199.93:16656,fab5dbc3af7d6ae92cf6da254de884e51300d529@147.182.155.210:33656,30b8f52a13f4c8780b0e5e432419d986e0378fee@193.46.243.184:26656,2e4e0f43100b424dc4b27e478acc39bebe32344d@77.37.176.99:55656,1026fa01cbf0640bb2f2cb20e253997bf51c1858@85.173.113.198:20656,daadbd8d2a477071d58874432c368a0f1a740129@38.242.202.234:26656,5baf972a3c97f9fd7eae5790e72cefeab37509b3@176.126.87.225:26656,394b18ba322e80876824463be71ed21a6878308c@38.242.203.139:26656,5e76a43265dad6321d7b67423792c847edfa5a1a@38.242.202.174:26656,59a370e401c64a3df8dfb245a7e58b28bc1c2df0@38.242.152.170:26656,2bbf9be5f935c9ee93ddf6b845babacb50fc67aa@147.182.151.134:33656,133e71b574df8f54f9ff6ba0347db8aebfba09ed@154.53.62.88:26656,525e0407588620337b43a72f79e683043c119d40@46.101.45.155:26656"
sed -i 's|^persistent_peers *=.*|persistent_peers = "'$peers'"|' $HOME/.terp/config/config.toml
sudo systemctl restart terpd
sudo journalctl -u terpd -f --no-hostname -o cat
```


