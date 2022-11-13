# monero-full-node

Simple docker image to run a monero full network node. 

This image and code is originally from [`kannix/monero-full-node`](https://github.com/kannix/monero-full-node) and has only been updated to use the latest version of Monero (currently `0.18.1.2` aka `Fluorine Fermi`) sourced from https://www.getmonero.org/downloads/#cli

Note: This container is compatible with Unraid, in case that matters to you.

## Tags
- `latest`: The latest version Monero (Currently `0.18.1.2`)
- [`0.18.1.2`: 29 Sept 2022](https://www.getmonero.org/2022/09/29/monero-GUI-0.18.1.2-released.html)
- `0.17.2.3`: Identical to the latest image provided by `kannix/monero-full-node` on 26 April 2021

# October 2018: Breaking Change
**warning**  
for improved security the new images will run the monero daemon under it's own user and not as root anymore!
If you simply upgrade without following the next steps you will run into this error:  
`WARN 	blockchain.db.lmdb	src/blockchain_db/lmdb/db_lmdb.cpp:75	Failed to open lmdb environment: Permission denied`

this can be fixed with the following steps  

* stop and remove the current container: `docker stop monerod && docker rm monerod`
* change the owner of the volume to monero user `docker run -v xmrchain:/home/monero/.bitmonero -t --rm --name=monerod -u root --entrypoint=/bin/chown ostlerdev/monero-full-node -R monero:monero .bitmonero`
* start the container `docker run -tid --restart=always -v xmrchain:/home/monero/.bitmonero -p 18080:18080 -p 18081:18081 --name=monerod ostlerdev/monero-full-node`

**Hint:** keep in mind that you have to adapt your volume bindings to your own configuration e.g. if you followed the older version of this readme you have to use: `-v /var/data/blockchain-xmr:/home/monero/.bitmonero` instead of `-v xmrchain:/home/monero/.bitmonero`

# Usage

`docker run -tid --restart=always -v xmrchain:/home/monero/.bitmonero -p 18080:18080 -p 18081:18081 --name=monerod ostlerdev/monero-full-node`

## Updates
Manual Way
```
docker stop monerod
docker rm monerod
docker pull ostlerdev/monero-full-node
```
Then launch using the "how to use" command above

Automatic way
https://github.com/v2tec/watchtower

# Donations

To donate to the original creator, please visit: https://github.com/kannix/monero-full-node 
