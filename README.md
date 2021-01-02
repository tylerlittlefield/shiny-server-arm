# shiny-server-arm

Run the following to get this working on a raspberry pi:

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

# build image
docker build --rm https://github.com/tylurp/shiny-server-arm.git --tag shiny-server-arm
```

Once complete, run `docker image ls` and you should see something similar to:

```bash
REPOSITORY             TAG       IMAGE ID       CREATED       SIZE
shiny-server-arm       latest    fca4fc467885   3 hours ago   1.23GB
ubuntu                 20.10     a5e86bd25736   5 weeks ago   71.9MB
```

And also, `docker container ls`:

```bash
CONTAINER ID   IMAGE                  COMMAND                  CREATED       STATUS       PORTS                    NAMES
a1d840b779ea   rstudio-shiny-server   "/etc/shiny-server/i…"   3 hours ago   Up 3 hours   0.0.0.0:3838->3838/tcp   affectionate_shannon
```

Start adding shiny apps with:

```bash
cd ~/shiny-server/apps
git clone <your app here>
```

Then navigate to `http://<your ip>:3838/`.