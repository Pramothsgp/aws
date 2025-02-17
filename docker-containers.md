
# DOCKER commands 

### insatll docker
``` bash []
yum install docker

```

### Start docker 

``` bash []
systemctl start docker
systemctl enable docker
```

### Ubuntu container

``` bash 
docker pull ubuntu
```

``` bash []
docker run -it ubuntu bash
```

`-it` creates an interactive teminal

**this enters into the ubuntu container**

### update
``` bash []
sudo apt-get update 
```

## check it has a public access
### install ping
``` bash []
apt-get install iputils-ping -y
```


## nginx container

``` bash []
docker pull nginx
```


