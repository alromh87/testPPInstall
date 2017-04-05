# Install WordPress
For this step we present 2 options
## Using existing linux system (Debian/Ubuntu)
## Using a Docker image from TurnKey Linux
This option is good to create a completly isolated environment just for development, and can be done in any [Docker supported plataform](https://docs.docker.com/engine/installation/#platform-support-matrix)

First you will need Docker installed, you can follow [this](https://docs.docker.com/engine/installation/linux/debian/#install-using-the-repository) instruction 

```bash
docker pull turnkeylinux/wordpress-14.1
CID=$(docker run -i -t -d turnkeylinux/wordpress-14.1)
CIP=$(docker inspect --format='{{.NetworkSettings.IPAddress}}' $CID)
docker logs $CID | grep "Random initial root password"
ssh root@$CIP
```

At this point you get asked to configure your new server and Wordpress instance, then you can go to your brand new Worpress site by visiting you VM's ip address form the browser.
