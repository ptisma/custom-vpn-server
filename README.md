# custom-vpn-server

Wanted to setup a simple VPN server running in docker container on one of my VM-s so I can use it and connect to it from my work laptop or phone when abroad.

Experimented with wireguard.

## wireguard
This folder contains the necessary code to setup the wireguard server.


### Setup for local
docker-compose.yml is a simple compose file for local hosting.

Run it with:
docker-compose up -d
When you have started the WireGuard container, it should automatically create all configuration files in your ./config folder. All you need to do is to copy the corresponding ./config/peer1/peer1.conf file to your client and use that as your wg0.conf, for instance.

To add additional clients:
Increase the PEERS parameter
docker-compose up -d --force-recreate

### Setup for Docker Swarm
docker-compose-swarm.yml is a compose file intended to host it as docker stack on the swarm cluster. After it's being hosted on one of the nodes, follow the previous steps. Only difference is, we do not explicitly set the server url at the beggining and for one of the non kernel volumes we are using the nfs storage which all nodes share.

Run it on one of the nodes:
docker stack deploy -c docker-compose-swarm.yml wireguard-stack

Install the QR code tool:
sudo apt-get install catimg

Get the config for the client (e.g ios client):
catimg /mnt/storage/opt/wireguard-server/config/peer1/peer1.png

Docs:
https://github.com/christianlempa/videos/tree/main/wireguard-docker
