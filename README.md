# Monero-on-BTCPay
A short guide on getting Monero setup on BTCPay

*Please note that Monero is considered a "Server-wide" implementation, and will not work on multi-store setups, unless all stores are receiving Monero to the same wallet. There can only be 1 wallet per SERVER, not per store like BTC based coins. 

## Choosing a server

Monero requires a approx 60GB of hard disk space at the time of writing this guide. You should ensure that the hard drive has sufficient space for growth, or is on a virtual server that allows easy resizing. 

For the purpose of this tutorial, I built this server on a [Digital Ocean Droplet](https://m.do.co/c/7f4facf9e1c0) <-- (Referral link, Get $100 credit if you spend $25) If you plan on adding BTC or any number of other coins, the storage costs on a service like Digital ocean might get excessive, and you might be best to switch to dedicated hardware with a service such as OVH. 

With any setup, you will want to make sure that you are using an SSD for hard drive space, and not an HDD. SSD's allow for quicker invoice generation and syncing of the blockchain, and ultimately a better customer experience. 

You will be installing the service using Docker during this guide, so please make sure your server also is docker compatible. 

## Setting up a DNS record

You will need to create a DNS record before setting up your server. This should be something like "btcpay.MYDOMAIN.com", where your domain is MYDOMAIN. If you are not familiar with how to do this, ask your web host or registrar. 

## Installing BTCPay

A lot of this guide is based on my BTCPay guide written here: https://medium.com/p/7a497339fb2f

Assuming you have purchased a server or droplet, you will be emailed some credentials (username, root password). (This assumes you know how to login to servers with SSH. If you are using a windows machine, try [PuTTY](https://www.putty.org/))

![1_5EN1V-d4jgGTmSF1vBLe0w](https://user-images.githubusercontent.com/3916318/166809273-ba4d350f-368c-4de6-af11-8ccd5c5cde52.png)

You will be asked to change your password immediately.

One you have logged into the new droplet via SSH, simply run the following commands as root:

```
mkdir BTCPayServer
cd BTCPayServer
git clone https://github.com/btcpayserver/btcpayserver-docker
cd btcpayserver-docker
```

This will make a BTCPay directory and download the docker files into it.

Here is a breakdown of each line we are going to add next:

Full code:

```
export BTCPAY_HOST="btcpay-sample.EXAMPLE.com"
export NBITCOIN_NETWORK="mainnet"
export BTCPAYGEN_CRYPTO1="xmr"
export BTCPAYGEN_REVERSEPROXY="nginx"
export BTCPAY_ENABLE_SSH=true
```

### export BTCPAY_HOST=”btcpay-sample.EXAMPLE.com”
This add’s the hostname “btcpay-sample.EXAMPLE.com”. You will want to replace this with your own hostname.

### export NBITCOIN_NETWORK=”mainnet”
This specifies we are building a mainnet node. Only change this if you are planning on using testnet.

### export BTCPAYGEN_CRYPTO1=”xmr”
This tells BTCPay we are setting up an XMR Node. Typically you would put "BTC" here for a BTC node, but for this tutorial we are doing XMR. If you would like both BTC and XMR use the following instead:

```
export BTCPAYGEN_CRYPTO1="btc"
export BTCPAYGEN_CRYPTO1="xmr"
```

and if you wish to enabled lightning on btc, select either lnd or clightning:

`export BTCPAYGEN_LIGHTNING=”lnd”` or `export BTCPAYGEN_LIGHTNING=”clightning”`

### export BTCPAYGEN_REVERSEPROXY=”nginx”
This tells BTCPay we will be using nginx as a reverse proxy. Don’t change unless you know what you are doing.

### export BTCPAY_ENABLE_SSH=true
This command does two things:

The Update button in ServerSettings/Maintenance will work properly
It allows you to add your own SSH keys to your server if you have access to btcpayserver admin account



There are more options you may set, however I would suggest checking the BTCPay docs for a full list. 



## Installing Keys

To use Monero on BTCPay, you need to add a view only wallet, and view only keys file to the wallet in BTCPay. For instructions on how to generate these files for specific wallets, please see the list below. 

1. Once your key files are generated, navigate to the BTCPay store you wish to enable Monero for. 
2. Click the Settings link (under Dashboard). 
3. Select the "Monero" tab.
4. Click "Modify" under the actions column.
5. Upload the view only wallet file. 
6. Upload the view only wallet .keys file. *Note, DO NOT UPLOAD THE WALLET KEYS, ONLY THE VIEW-ONLY keys*
7. If a password is on your wallet keys file, indicated it here and save. 
8. Click "Enable"

Once the files have been uploaded, your wallet will likely restart. 

You will likely see the following Status:
Node available: True
Wallet available: False (Wallet file present)

This may take several minutes for the Wallet Available to become "True"

If your wallet does not restart and continues to show false, I would suggest restarting the docker instance, or btcpay in its entirety. 

### - Feather Wallet
I've found Feather Wallet to be my goto wallet, as it allows for things such as coin control. This can be useful in some cases, and segregating funds if needed. 



