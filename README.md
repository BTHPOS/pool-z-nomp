# Bithereum Mining Pool

Feel free to run your own bithereum pool if you have the resources and expertise to handle the work involved in maintaining one. The code within this repo is a forked version of z-nomp modified for Equihash coins. 

#### Requirements
* Bithereum daemon (refer to [Bithereum's core repo](https://github.com/BTHPOS/BTH))
* [Node.js](http://nodejs.org/) v7 ([How to install node](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager))
* [Redis](http://redis.io/) key-value store v2.6+ ([How to install redis](http://redis.io/topics/quickstart))

Make sure you have done all the requirements before continuing. If you use old versions of Node.js or Redis that may come with your system package manager then you will have problems. Follow the linked instructions to get the last stable versions.

## Step 1: Setting up coin daemon
Follow the installation instructions for your bithereum daemon. Your bithereum.conf file (usually located in ~/.bithereum/ folder by default) should end up looking something like the following.
```conf
txindex=1
daemon=1
rpcuser=bithereum
rpcpassword=bithereum
rpcport=8332
```

## Step 2: Downloading & Installing
Clone this repository run the following commands from within the root level of the cloned folder. Alternatively, you can install this pool by using the pool [installation script here](https://github.com/BTHPOS/installation-scripts).

```bash
$ sudo apt-get update -y
$ sudo apt-get install npm -y
$ sudo npm install n -g -y
$ sudo n v7
$ sudo apt update
$ sudo apt install redis-server -y
$ git clone https://github.com/BTHPOS/pool-z-nomp.git pool
$ cd pool
$ npm update
$ npm install
```

## Step 3: Configure Pool
Take a look at the example json file inside the `pool_configs` directory. Rename it to `bithereum.json` and change the
example fields to fit your setup.

## Step 4: Start the Pool

```bash
npm start
```

## Additional Notes

* [Redis security warning](http://redis.io/topics/security): be sure firewall access to redis - an easy way is to
include `bind 127.0.0.1` in your `redis.conf` file. Also it's a good idea to learn about and understand software that
you are using - a good place to start with redis is [data persistence](http://redis.io/topics/persistence).

* For redundancy, its recommended to have at least two Bithereum daemon instances running in case one drops out-of-sync or offline,
all instances will be polled for block/transaction updates and be used for submitting blocks. Creating a backup daemon
involves spawning a daemon using the `-datadir=/backup` command-line argument which creates a new daemon instance with
it's own config directory and coin.conf file. Learn about the daemon, how to use it and how it works if you want to be
a good pool operator. For starters be sure to read:
   * https://en.bitcoin.it/wiki/Running_bitcoind
   * https://en.bitcoin.it/wiki/Data_directory
   * https://en.bitcoin.it/wiki/Original_Bitcoin_client/API_Calls_list
   * https://en.bitcoin.it/wiki/Difficulty
   
* Use something like [forever](https://github.com/nodejitsu/forever) to keep the node script running
in case the master process crashes. 

* Use something like [redis-commander](https://github.com/joeferner/redis-commander) to have a nice GUI
for exploring your redis database.

* Use something like [logrotator](http://www.thegeekstuff.com/2010/07/logrotate-examples/) to rotate log 
output from Z-NOMP.

* Use [New Relic](http://newrelic.com/) to monitor your Z-NOMP instance and server performance.

## Setting up blocknotify (Optional)
1. In `config.json` set the port and password for `blockNotifyListener`
2. In your daemon conf file set the `blocknotify` command to use:
```
node [path to cli.js] [coin name in config] [block hash symbol]
```
Example: inside `zclassic.conf` add the line
```
blocknotify=node /home/user/z-nomp/scripts/cli.js blocknotify zclassic %s
```

Alternatively, you can use a more efficient block notify script written in pure C. Build and usage instructions
are commented in [scripts/blocknotify.c](scripts/blocknotify.c).

## Upgrading Z-NOMP
When updating Z-NOMP to the latest code its important to not only `git pull` the latest from this repo, but to also update
the `node-stratum-pool` and `node-multi-hashing` modules, and any config files that may have been changed.
* Inside your Z-NOMP directory (where the init.js script is) do `git pull` to get the latest Z-NOMP code.
* Remove the dependenices by deleting the `node_modules` directory with `rm -r node_modules`.
* Run `npm update` to force updating/reinstalling of the dependencies.
* Compare your `config.json` and `pool_configs/coin.json` configurations to the latest example ones in this repo or the ones in the setup instructions where each config field is explained. <b>You may need to modify or add any new changes.</b>

Credits
-------
### Bithereum
* [Dondrey Taylor / dondreytaylor](https://github.com/dondreytaylor)

### Z-NOMP
* [Joshua Yabut / movrcx](https://github.com/joshuayabut)
* [Aayan L / anarch3](https://github.com/aayanl)
* [hellcatz](https://github.com/hellcatz)

### NOMP
* [Matthew Little / zone117x](https://github.com/zone117x) - developer of NOMP
* [Jerry Brady / mintyfresh68](https://github.com/bluecircle) - got coin-switching fully working and developed proxy-per-algo feature
* [Tony Dobbs](http://anthonydobbs.com) - designs for front-end and created the NOMP logo
* [LucasJones](//github.com/LucasJones) - got p2p block notify working and implemented additional hashing algos
* [vekexasia](//github.com/vekexasia) - co-developer & great tester
* [TheSeven](//github.com/TheSeven) - answering an absurd amount of my questions and being a very helpful gentleman
* [UdjinM6](//github.com/UdjinM6) - helped implement fee withdrawal in payment processing
* [Alex Petrov / sysmanalex](https://github.com/sysmanalex) - contributed the pure C block notify script
* [svirusxxx](//github.com/svirusxxx) - sponsored development of MPOS mode
* [icecube45](//github.com/icecube45) - helping out with the repo wiki
* [Fcases](//github.com/Fcases) - ordered me a pizza <3
* Those that contributed to [node-stratum-pool](//github.com/zone117x/node-stratum-pool#credits)

License
-------
Released under the MIT License. See LICENSE file.
