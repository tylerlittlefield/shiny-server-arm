# shiny-server-arm

```bash
# give user root access to docker
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker pirate

# build image
docker build https://github.com/tylurp/shiny-server-arm.git --tag shiny-server-arm

# create directories
cd ~
mkdir shiny-server
mkdir shiny-server/logs
mkdir shiny-server/conf
mkdir shiny-server/apps

# bind directories
docker volume create --name shiny-apps --opt type=none --opt device=/home/pirate/shiny-server/apps/ --opt o=bind
docker volume create --name shiny-logs --opt type=none --opt device=/home/pirate/shiny-server/logs/ --opt o=bind
docker volume create --name shiny-conf --opt type=none --opt device=/home/pirate/shiny-server/conf/ --opt o=bind

# build image
docker build --rm https://github.com/tylurp/shiny-server-arm.git --tag shiny-server-arm
```