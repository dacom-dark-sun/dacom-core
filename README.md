
# TravelChain Github Readme

## Introduction

TravelChain Core is the BitShares-based blockchain implementation and command-line interface. The web wallet is TravelChain UI.

Visit travelchain.io to learn about TravelChain and join the community at Telegram. Visit BitShares.org to learn about BitShares.

Existing repositories can be updated with the following steps:

```
git remote set-url origin https://github.com/TravelChain/travelchain-core.git
git checkout master
git remote set-head origin --auto
git pull
git submodule sync --recursive
git submodule update --init --recursive

```

## Getting started
#### For OS X
Install XCode and its command line tools by following the instructions here: https://guide.macports.org/#installing.xcode. In OS X 10.11 (El Capitan) and newer, you will be prompted to install developer tools when running a developer command in the terminal. This step may not be needed.
Install Homebrew by following the instructions here: http://brew.sh/
Initialize Homebrew:

```
brew doctor
brew update

```

### Install dependencies:

```
brew install boost cmake git openssl autoconf automake 
brew link --force openssl 

```

Optional. To support importing Bitcoin wallet files:

```
brew install berkeley-db

```

Optional. To use TCMalloc in LevelDB:

```
brew install google-perftools

```

#### Clone the TravelChain repository:

```
git clone https://github.com/travelchain/travelchain-core.git
cd graphene

```

### Build TravelChain:

```
git submodule update --init --recursive
cmake -DCMAKE_BUILD_TYPE=Release .
make

```
##### For Ubuntu


###### Ubuntu 14.04 & 16.04
Ubuntu 14.04 & 16.04 Build and Install Instructions
The following dependencies were necessary for a clean install of Ubuntu 14.04 LTS:

```
sudo apt-get update
sudo apt-get install cmake make libbz2-dev libdb++-dev libdb-dev libssl-dev openssl libreadline-dev autoconf libtool git ntp libcurl4-openssl-dev g++

```

Build Boost 1.57.0 (for Ubuntu 16.04 this step can be skipped)
The Boost which ships with Ubuntu 14.04 is too old. You need to download the Boost tarball for Boost 1.57.0 (Note, 1.58.0 requires C++14 and will not build on Ubuntu 14.04 LTS; this requirement was an accident, see this mailing list post).

```
BOOST_ROOT=$HOME/opt/boost_1_57_0
sudo apt-get update
sudo apt-get install autotools-dev build-essential libbz2-dev libicu-dev python-dev
wget -c 'http://sourceforge.net/projects/boost/files/boost/1.57.0/boost_1_57_0.tar.bz2/download' -O boost_1_57_0.tar.bz2
[ $( sha256sum boost_1_57_0.tar.bz2 | cut -d ' ' -f 1 ) == "910c8c022a33ccec7f088bd65d4f14b466588dda94ba2124e78b8c57db264967" ] || ( echo 'Corrupt download' ; exit 1 )
tar xjf boost_1_57_0.tar.bz2
cd boost_1_57_0/
./bootstrap.sh "--prefix=$BOOST_ROOT"
./b2 install

```

Ubuntu 16.04 LTS ships with Boost 1.58 libraries, so no need to build from source.

```
sudo apt-get install libboost-all-dev
```

### Build Travelchain Core

```
cd ..
git clone https://github.com/travelchain/travelchain-core.git
cd travelchain-core
git submodule update --init --recursive
cmake -DCMAKE_BUILD_TYPE=Release .
make

``` 
 
###### Windows - Visual Studio 2013

Prerequisites
Microsoft Visual C++ 2013 Update 1 (the free Express edition will work)
If you have multiple MSVS installation use MSVS Developer console from target version.
This build is for 64bit binaries.
Set up the directory structure
Create a base directory for all projects. I'm putting everything in D:\travelchain, you can use whatever you like. In several of the batch files and makefiles, this directory will be referred to as GRA_ROOT:

```
mkdir D:\travelchain

```
How To become an active witness in TravelChain

Set in the config default seed-nodes.txt, then will start the synchronization with the working network
 
get_account <my name>
config: witness-id = "1.6.10"
private-key = ["GPH7vQ7GmRSJfDHxKdBmWMeDMFENpmHWKn99J457BNApiX1T5TNM8","5JGi7DM7J8fSTizZ4D9roNgd8dUc5pirUe9taxYCUUsnvQ4zCaQ"]

To become a witness and be able to produce blocks, you first need to create a witness object that can be voted in.
We create a new witness object by issuing::


```
>>> create_witness "http://" true { "ref_block_num": 139, "ref_block_prefix": 3692461913, "relative_expiration": 3, "operations": [[ 21,{ "fee": { "amount": 0, "asset_id": "1.3.0" }, "witness_account": "1.2.16", "url": "url-to-proposal", "block_signing_key": "", "initial_secret": "00000000000000000000000000000000000000000000000000000000" } ] ], "signatures": [ ˓→"1f2ad5597af2ac4bf7a50f1eef2db49c9c0f7616718776624c2c09a2dd72a0c53a26e8c2bc928f783624c4632924330fc03f08345c8f40b9790efa2e4157184a37 ˓→" ] }

```

Our witness is registered, but it can’t produce blocks because nobody has voted it in. You can see the current list of active witnesses with get_global_properties:: >>> get_global_properties { "active_witnesses": [ "1.6.0", "1.6.1", "1.6.2", "1.6.3", "1.6.4", "1.6.5", "1.6.7", "1.6.8", "1.6.9" ], ... Now, we should vote our witness in. Vote all of the shares your account in favor of your new witness.: >>> vote_for_witness true true [a transaction in json format] 


Note: If you want to experiment with things that require voting, be aware that votes are only tallied once per day at the maintenance interval. get_dynamic_global_properties tells us when that will be in next_maintenance_time. Once the next maintenance interval passes, run get_global_properties again and you should see that your new witness has been voted in. Now we wait until the next maintenance interval. 


## Graphene CLI Wallet Cookbook

Running a Local Test Network
Right now, there is no public testnet, so the only way to test is to run your own private network. To do this, launch a witness node to generate blocks. In the directory where you built your graphene distribution:
cd programs/witness_node
* if you have previously run a witness node, you may need to remove the old blockchain.
* at this early stage, new commits often make it impossible to reuse an old database

```
 rm -r witness_node_data_dir
./witness_node --rpc-endpoint "127.0.0.1:8090" --enable-stale-production -w \""1.6.0"\" \""1.6.1"\" \""1.6.2"\" \""1.6.3"\" \""1.6.4"\"

```


The initial genesis state has ten pre-configured delegates (1.6.0-9) that all use the same private key to sign their blocks, and the witness node has the private keys for these initial delegates built in.. Launching witness_node this way allows you to act as all ten delegates.
Now, in a second window, launch a cli_wallet process to interact with the network.
cd programs/cli_wallet
* similarly, if you have previously run a wallet, you may need to wipe out your 
* old wallet
``` rm wallet.json
./cli_wallet

```

Before doing anything with the new wallet, set a password and unlock the wallet.
Warning: your passwords will be displayed on the screen.
new >>> set_password my_password
locked >>> unlock my_password
unlocked >>>

#### Support 
Technical support is available in the Telegram Chain.
TravelChain Core bugs can be reported directly to the issue tracker.
TravelChain UI bugs should be reported to the UI issue tracker

#### API
You can restrict API's to particular users by specifying an api-access file in config.ini or by using the --api-access /full/path/to/api-access.json startup node command. Here is an example api-access file which allows user bytemasterwith password supersecret to access four different API's, while allowing any other user to access the three public API's necessary to use the wallet:

```
{
   "permission_map" :
   [
      [
         "bytemaster",
         {
            "password_hash_b64" : "9e9GF7ooXVb9k4BoSfNIPTelXeGOZ5DrgOYMj94elaY=",
            "password_salt_b64" : "INDdM6iCi/8=",
            "allowed_apis" : ["database_api", "network_broadcast_api", "history_api", "network_node_api"]
         }
      ],
      [
         "*",
         {
            "password_hash_b64" : "*",
            "password_salt_b64" : "*",
            "allowed_apis" : ["database_api", "network_broadcast_api", "history_api"]
         }
      ]
   ]
}

```


Passwords are stored in base64 as salted sha256 hashes. A simple Python script, saltpass.py is avaliable to obtain hash and salt values from a password. A single asterisk "*" may be specified as username or password hash to accept any value.
With the above configuration, here is an example of how to call add_node from the network_node API:

```
{"id":1, "method":"call", "params":[1,"login",["bytemaster", "supersecret"]]}
{"id":2, "method":"call", "params":[1,"network_node",[]]}
{"id":3, "method":"call", "params":[2,"add_node",["127.0.0.1:9090"]]}

```

Note, the call to network_node is necessary to obtain the correct API identifier for the network API. It is not guaranteed that the network API identifier will always be 2.
Since the network_node API requires login, it is only accessible over the websocket RPC. Our doxygen documentation contains the most up-to-date information about API's for the witness node and the wallet. If you want information which is not available from an API, it might be available from the database; it is fairly simple to write API methods to expose database methods.
License
TravelChain Core is under the MIT license. See LICENSE for more information.



