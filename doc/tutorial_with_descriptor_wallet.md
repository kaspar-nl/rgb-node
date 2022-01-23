# LNP/BP Descriptor Wallet
# LNP/BP rgb-node

- All commands are for `testnet`
- This tutorial is just a start to make it more easy for people to use the `descriptor wallet` and `rgb`(node)

## LNP/BP Descriptor Wallet
### Linux
#### Download & Install
```console
$ git clone https://github.com/LNP-BP/descriptor-wallet
$ cd descriptor-wallet
$ cargo install --path . --locked --all-features
```

#### Create a seed and write to file (can be used for testnet and mainnet):
```console
$ btc-hot seed wallet.seed
```

#### Create a (new) hot wallet 
Create a hot wallet (bip84) file based on the seed for signing transactions:
```console
$ btc-hot derive -s bip84 --testnet wallet.seed bip84_wallet_testnet
```

Optional: create a taproot hot wallet (bip86) file based on the seed for signing transactions:
```console
$ btc-hot derive -s bip86 --testnet wallet.seed tr_wallet_testnet
```

Result (example), this is the 'descriptor' that you will need later in the process to create a 'cold wallet':
- `wpkh(m=[f8184019]/84h/1/0h=[tpubDCny2AW1LgQoPkMUodFvadjKTKBoP7vGNCm8irYhBTLaKeuqyiqt7VzwF8J2akynwjcZ1iofi6iM4Nd6ey7Sfrg1ZTRLtrEFqcn9qEjeNLp]/*/*)#eykq8mdh`

Or for taproot wallet:
- `tr(m=[f8184019]/86h/1/0h=[tpubDC2X2CtAcNuj5KATTmXPs6xQxKVAL1Fc1cW2rfZV3xQtaHxsdPdKz7NLgK4xX6HKCk6WwUP3dU8powVPZfVRjZYRFe4rP77KgcF5SpV4rra]/*/*)#zv69kr4y`

#### Create a cold wallet
Create a cold wallet based on the hot wallet that was created in the previous process:
```console
$ btc-cold create "wpkh(m=[f8184019]/84h/1/0h=[tpubDCny2AW1LgQoPkMUodFvadjKTKBoP7vGNCm8irYhBTLaKeuqyiqt7VzwF8J2akynwjcZ1iofi6iM4Nd6ey7Sfrg1ZTRLtrEFqcn9qEjeNLp]/*/*)#eykq8mdh" bip84_coldwallet_testnet
```

or for the taproot wallet: 
```console
$ btc-cold create "tr(m=[f8184019]/86h/1/0h=[tpubDC2X2CtAcNuj5KATTmXPs6xQxKVAL1Fc1cW2rfZV3xQtaHxsdPdKz7NLgK4xX6HKCk6WwUP3dU8powVPZfVRjZYRFe4rP77KgcF5SpV4rra]/*/*)#zv69kr4y" tr_coldwallet_testnet
```
 
#### Show addresses from your wallet
Now you are able to show the (e.g. first 5) addresses belonging to the cold wallets:
```console
$ btc-cold address bip84_coldwallet_testnet -n 5
```
 
#### Show funds in your wallet
You can also investigate the amount of funds that are in the wallet:
```console
$ btc-cold check bip84_coldwallet_testnet
```
 
#### Transfer funds from your wallet (Create PSBT)
You can also transfer the funds from the wallet by using the so-called UTXOS (that are visible when you have funds) in the following command:
```console
$ btc-cold construct -i "<txid:vout> /<i>/<j> rbf" -o="<address>:<amount>" tr_coldwallet_testnet tx.psbt <fee>
```
or as an example:
```console
$ btc-cold construct -i "b1aef321281b296b5cbd87e599abd28196d481f2fcb10e6c5465cdd5afa7d8c7:1 /0/0 rbf" -o="tb1qhfs3p78zhhxxwzrkyuew4mzterqme9kfuyrrhe:99800" tr_coldwallet_testnet tx.psbt 200
```

#### Sign PSBT
This will create the PSBT file that needs to be signed, that can only be done with your HOT (signing) wallet by for example:
```console
$ btc-hot sign tx.psbt tr_wallet_testnet
```

#### Finalize and Broadcast
Next step is to finalize the PSBT to create the final transaction by using the cold wallet, and publish/broadcast it subsequently:
```console
$ btc-cold finalize --publish testnet tx.psbt
```
 
# LNP/BP rgb-node
### Linux:
#### Download & Install
```console
$ git clone https://github.com/LNP-BP/rgb-node.git
$ cd rgb-node
$ cargo install --all-features --bins --path .
```
#### Create fungible token linked to a UTXO (from descriptor wallet)
#### Create PSBT for the transfer
#### Request for blinded UTXO
#### Transfer the token to third party, and publish it 
#### Transfer the files to third party and finalze the transfer

