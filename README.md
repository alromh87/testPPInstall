# Install WordPress
For this step we present 2 options
## Using existing linux system (Ubuntu/debian)
## Using a docker image from 
This option is good to create a completly isolated environment just for development

```bash
docker pull turnkeylinux/wordpress-14.1
CID=$(docker run -i -t -d turnkeylinux/wordpress-14.1)
CIP=$(docker inspect --format='{{.NetworkSettings.IPAddress}}' $CID)
docker logs $CID | grep "Random initial root password"
ssh root@$CIP
```

At this point you get asked to configure your new server and worpress instance
