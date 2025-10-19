<div align="center">

# Cracked Nessus in Docker

</div>

<br>

Work and creds goes to [elliot-bia](https://github.com/elliot-bia/). Twitter: [Elliot58616851](https://twitter.com/Elliot58616851)

Do not use it for illegal purposes. If any infringement, please create a new issue with the title "Infringement".


## Setup
````
sudo apt install docker.io
sudo usermod -aG docker $USER   #Log out and back in after this
docker pull ramisec/nessus
docker run -d --name=nessus -p 8834:8834 ramisec/nessus
docker exec -it nessus /bin/bash /nessus/update.sh
````

## Migration

If you need to migrate from old versions to new, use the following commands:

```bash
#Crate dir
mkdir ~/nessus_data

#Stop container
docker stop nessus

#copy data
docker cp nessus:/opt/nessus/var/nessus/ ~/nessus_data

#delete old container
docker rm nessus

#run new container
docker run -itd --name=nessus -v ~/nessus_data/nessus/:/opt/nessus/var/nessus/ -p 8834:8834 ramisec/nessus

#update plugins
docker exec -it nessus /bin/bash /nessus/update.sh
```

## Username & Password

Username: `admin`

Reset password by running `docker exec -it nessus /opt/nessus/sbin/nessuscli chpasswd`

Example output:
```
Login to change: admin 
New password: Password123!
New password (again): Password123!
Password changed for admin
```

Now open your browser to https://localhost:8834


## Other

### Plugin update failed

sudo docker exec nessus sed -i 's|wget (https://plugins.nessus.org[^ ]*)|wget "\1"|' /nessus/update.sh

or

docker exec -it nessus /bin/bash /nessus/update.sh "https://plugins.nessus.org/v2/nessus.php?f=all-2.0.tar.gz&u=xxxxxxxxxxxxxxxxxxxxxxxx&p=xxxxxxxxxxxxxxxxxxx"





