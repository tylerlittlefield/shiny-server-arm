# shiny-server-arm

![build](https://github.com/tyluRp/shiny-server-arm/workflows/build/badge.svg)

## docker-compose

```bash
# install docker
curl -sSL https://get.docker.com | sh

# install docker-compose
sudo apt-get install libffi-dev libssl-dev -y
sudo apt-get install python3 python3-pip -y
sudo pip3 install --upgrade pip
sudo pip3 install docker-compose

# give user root access to docker, replace <user> with your username
sudo usermod -aG docker <user>

# clone repo
git clone https://github.com/tylurp/shiny-server-arm.git

# create directories
cd ~
mkdir shiny-server
mkdir shiny-server/logs
mkdir shiny-server/conf
mkdir shiny-server/apps

# from repo directory
docker-compose up -d
```

If you see the following error:

```bash
ERROR: An HTTP request took too long to complete. Retry with --verbose to obtain debug information.
If you encounter this issue regularly because of slow network conditions, consider setting COMPOSE_HTTP_TIMEOUT to a higher value (current value: 60).
```

Try running:

```bash
sudo service docker restart
docker-compose up -d
```

## Without docker-compose

```bash
# install docker
curl -sSL https://get.docker.com | sh

# give user root access to docker, replace <user> with your username
sudo usermod -aG docker <user>

# build image
docker build https://github.com/tylurp/shiny-server-arm.git --tag shiny-server-arm

# create directories
cd ~
mkdir shiny-server
mkdir shiny-server/logs
mkdir shiny-server/conf
mkdir shiny-server/apps

# bind directories, replace <user> with your username
docker volume create --name shiny-apps --opt type=none --opt device=/home/<user>/shiny-server/apps/ --opt o=bind
docker volume create --name shiny-logs --opt type=none --opt device=/home/<user>/shiny-server/logs/ --opt o=bind
docker volume create --name shiny-conf --opt type=none --opt device=/home/<user>/shiny-server/conf/ --opt o=bind

# run container
docker run -d -p 3838:3838 -v shiny-apps:/srv/shiny-server/ -v shiny-logs:/var/log/shiny-server/ -v shiny-conf:/etc/shiny-server/ --name shiny-server-arm tylurp/shiny-server-arm
```

## Add apps

Start adding shiny apps with:

```bash
cd ~/shiny-server/apps
git clone <your app here>
```

Then navigate to `http://<your ip>:3838/`.

[](index.png)

## Acknowledgements

A lot of the Dockerfile is based on existing work, please see [`hvalev/shiny-server-arm-docker`](https://github.com/hvalev/shiny-server-arm-docker).