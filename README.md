# webtop
Quick and easy way to get webtop up and running

# Intstall steps debian/ubuntu

### Install Docker and Git
```
sudo apt update && sudo apt upgrade
sudo apt install git
curl -fsSL https://get.docker.com -o install-docker.sh | sh
```

#### Optional, install ZSH and Oh My ZSH.  This gives a better shell and tab completion for many commands including docker
```
sudo apt install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Add yourself to the Docker group
```
sudo usermod -aG docker $(whoami)
```

### Get started
```
git clone https://github.com/mattheys/webtop
cd webtop
cp .env.example .env
```

### Edit the environment file and fill in the details

**DUCK_DNS_DOMAIN** This is the duckdns domain without the duckdns.org on the end

**DUCK_DNS_TOKEN** Your duckdns Api Token

**CADDY_USER** Username to access webtop

**CADDY_PASSWORD** Password to access webtop, password must be a bcrypt encrypted password https://bcrypt-generator.com/  The $'s in the password must be escapped with an extra $ e.g. $$2a$$12$$

### Go
```
docker compose --profile webtop up -d
```

#### Optional start Portainer to manage containers, you must access and setup the login information for portainer within a few minutes or the container will stop itself
```
docker compose --profile portainer up -d
```

You should now be able to access your webtop at https://webtop.{Domain}.duckdns.org

### All Profiles

- **all**  special profile to bring up everything

- dozzle
- nodered
- portainer
- webtop