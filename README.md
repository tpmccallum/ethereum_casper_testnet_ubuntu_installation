# Installing Casper on Ubuntu 16.04 LTS
Disclaimer - see Appendix B at the end of this document.

## Housekeeping

`
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install curl
`

## Docker

`
curl -fsSL get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker $USER
sudo shutdown -r now
`

## Git

`
sudo apt-get install git
`
## PIP

`
sudo apt-get install python3-pip
`

## Web3 
Please use the -H (home) argument for sudo

`
sudo -H pip3 install web3
`

Fetch casper docker containers from @karlfloersch's github

`
git clone http://github.com/karlfloersch/docker-pyeth-dev

cd docker-pyeth-dev

make new-account
`

# Running

To start Casper try command A OR command B below (if you need to restart/rerun please see appendix A at the end of this doc)

A

`
make run-node bootstrap_node=enode://d3260a710a752b926bb3328ebe29bfb568e4fb3b4c7ff59450738661113fb21f5efbdf42904c706a9f152275890840345a5bc990745919eeb2dfc2c481d778ee@54.167.247.63:30303
`

B

`
make run-node bootstrap_node=enode://a120401858c93f0be73ae7765930174689cad026df332f7e06a047ead917cee193e9210e899c3143cce55dd991493227ecea15de42aa05b9b730d2189e19b567@52.87.179.32:30303
`

# Interacting using Web3

Open a new terminal 

`
ctrl + shift + t 
`

Get your local unique casper docker container id by typing

`
docker ps
`

Use your docker id from the previous step in the next command by replacing replaceMeWithId

`
docker exec -it replaceMeWithId python
`

Type these commands at the Python prompt

`
from web3 import Web3, HTTPProvider
web3 = Web3(HTTPProvider('http://localhost:8545'))
web3.eth.getBlock('latest')
`
# Getting testnet ETH
Before you can run a validator you need to have at least 1500 testnet ETH. You can get testet ETH from a faucet or earn it by mining.

## Faucet - do not use
Install docker compose

`
sudo apt install docker-compose
`
Edit the yaml file (change the version on the first line to 2.0)

`
vi docker-compose.yml
`

Run the following command
`
docker-compose up
`
Open a new terminal and run the following commands

`
docker cp utils/faucet_util.py bootstrap:/ethereum/
docker exec -it bootstrap bash
`
## Faucet - do not use




## Mining
The following command will start your node mining for ETH. If you have problems stopping and starting the docker process please see Appendix A at the end of this document.

`
make run-node mine_percent=90 bootstrap_node=enode://d3260a710a752b926bb3328ebe29bfb568e4fb3b4c7ff59450738661113fb21f5efbdf42904c706a9f152275890840345a5bc990745919eeb2dfc2c481d778ee@54.167.247.63:30303
`

# Checking accounts and account balances

Get your coinbase account balance using the following command at the Python prompt. The coinbase account is where validation rewards are paid

`
web3.eth.getBalance(web3.eth.coinbase)
`

you can look at all accounts by using the following commands

`
web3.eth.accounts
`

this outputs an array as follows.

`
web3.eth.accounts
['0xcFA7032aF32A5200023332E141BaE61f944DE28D', '0xcFA7032aF32A5200023332E141BaE61f944DE28D']
`

If you look closely you will see that the web3.eth.coinbase argument which we used above produces the same address as the first element in the array i.e.

`
web3.eth.coinbase
'0xcFA7032aF32A5200023332E141BaE61f944DE28D'
`

of course, as the coinbase address is the first element in the array, it can also be obtained by using the following command.

`
web3.eth.accounts[0]
'0xcFA7032aF32A5200023332E141BaE61f944DE28D'
`
This syntax introduces the idea that we can obtain the other address by using commands like the following

`
web3.eth.accounts[1]
`

The above outputs the address

`
'0xcFA7032aF32A5200023332E141BaE61f944DE28D'
`

and in turn we are able to obtain the balance of this account like this

`
web3.eth.getBalance(web3.eth.accounts[1])
`

or like this

`
web3.eth.getBalance('0xcFA7032aF32A5200023332E141BaE61f944DE28D')
`
# Validating

The validating (running a validator) section will be tested/documented once I have enough testnet ETH for a deposit. I am both mining and asking for testnet ETH from the community.


# Appendix A
Stopping the docker process from running seems a bit problematic. When hitting ctrl + c you are asked to hit ctrl + c again to terminate or "enter" to enter the console mode. In my experience repeating ctrl + c did not work. I now chose to hit ctrl + c once then "enter" once to access the console mode. Once I am in the console I type quit and this gets me back to my Ubuntu command line where I can clean up using the following step.

Once you have stopped the docker process you will need to run the following command before restarting.

`
docker rm validator
`
# Appendix B
This work is only intended for myself at this stage. It is a record of my personal experience installing Casper today. This work is not endorsed. It is not recommended for use yet. The content of this work is based on https://hackmd.io/s/Hk6UiFU7z by @karlfloersch < https://github.com/karlfloersch >.
