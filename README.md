# Dev environment for Dave Hakkens Community Site, Worpress API, setup
## WordPress base instalation
For this step we present 2 options
### Using existing linux system (Debian/Ubuntu)
### Using a Docker image from TurnKey Linux
This option is good to create a completly isolated environment just for development, and can be done in any [Docker supported plataform](https://docs.docker.com/engine/installation/#platform-support-matrix)

First you will need Docker installed, you can follow [this](https://docs.docker.com/engine/installation/linux/debian/#install-using-the-repository) instructions

After having Docker installed we need to download Turnkey Linux WordPress image and start the container, for this we store container's IP in $CID and container's IP in $CIP, also note the self generated password that we will be using to ssh into de container.

```bash
docker pull turnkeylinux/wordpress-14.1
CID=$(docker run -i -t -d turnkeylinux/wordpress-14.1)
CIP=$(docker inspect --format='{{.NetworkSettings.IPAddress}}' $CID)
docker logs $CID | grep "Random initial root password"
ssh root@$CIP
```

At this point you get asked to configure your new server and WordPress instance, then you can go to your brand new WordPress site by visiting your VM's IP address form the browser.

For the API we will be using REST API, that is included from WP 4.7, since docker image comes with WP 4.4 it's requiered to update via WP admin page, login as admin and upgrade by clicking at the mesaage at the lef top corner.

//TODO: add image

#### Restarting configured instance
In case you shut down your container you will need to re-launch it, first by finding it's ID

```bash
docker ps -a
```
CONTAINER ID | IMAGE | COMMAND | CREATED | STATUS | PORTS | NAMES
------------ | ------------- | ------------ | ------------- | ------------ | ------------- | ------------
**3b862c99b491** | turnkeylinux/wordpress-14.1 | "/usr/sbin/start.sh" | 22 hours ago | Exited (137) 21 hours ago |  | zealous_khorana

and using this id to start and connect to it using your setup password:

```bash
CID=$(docker restart 3b862c99b491)
CIP=$(docker inspect --format='{{.NetworkSettings.IPAddress}}' $CID)
ssh root@$CIP
```

## Configuring WordPress

### Install requiered  Plugins
Log in to WP admin console: Plugins -> Add New, install and activate requiered Pugins
- BBpress
- Buddypress
- myCRED
- GD bbPress Attachments

### Install DAVE COMMUNITY Theme
Go to WP themes directory and clone github repo

```bash
cd /var/www/wordpress/wp-content/themes/
git clone https://github.com/hakkens/davehakkens.git
```
Now enable theme from WP admin console: Appearance -> Themes : Dave Hakkens 2.0

### Enable clean URL's

```bash
a2enmod rewrite
/etc/init.d/apache2 restart
```

## Using WP REST API

API is accesible at wp-json/wp/v2/

Dev commit ( alromh87/precious-plastic-map@d3c39abc851a3cf4352df20c82ceccce6a5ca7bc ) adds 3 extra endpoints 
- map_pins: For pins
- ppmap_pin_service: Taxonomy for available services
- ppmap_pin_tag: Taxonomy for hashtags
