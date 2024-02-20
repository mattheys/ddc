# Dynamic Docker Caddy
Quick and easy way to get a reverse proxy up and running

# Intstall steps debian/ubuntu

## Install Docker and Git
```
sudo apt update && sudo apt upgrade && \
sudo apt install git curl && \
curl -fsSL https://get.docker.com | sh
```

### Optional, install ZSH and Oh My ZSH.  This gives a better shell and tab completion for many commands including docker
```
sudo apt install zsh && \
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## Add yourself to the Docker group
```
sudo usermod -aG docker $(whoami)
```

## Get started
```
git clone https://github.com/mattheys/ddc && \
cd ddc && \
cp .env.example .env && \
cp authelia/sites.example.yml authelia/sites.yml && \
cp authelia/users.example.yml authelia/users.yml
```

## Edit the .env environment file and fill in the details

**DUCK_DNS_DOMAIN** This is the duckdns domain without the duckdns.org on the end

**DUCK_DNS_TOKEN** Your duckdns Api Token

**AUTHELIA_JWT_SECRET**, **AUTHELIA_SESSION_SECRET**, **AUTHELIA_STORAGE_ENCRYPTION_KEY** These are required to secure Authelia and should be different, you can generate these with the following command `docker run --rm authelia/authelia:latest authelia crypto rand --length 64 --charset alphanumeric`

*Optionally* fill in the lines that start with **AUTHELIA_NOTIFIER_SMTP** to enable SMTP notifications to users in order to allow things like 2FA to be setup.

## Update users.yml

Add your users to the users.yml file, to generate the password you can run `docker run -it --rm authelia/authelia:latest authelia crypto hash generate argon2` 

## Update sites.yml

Update the sites.yml with your domain name, configure specific subdomains with different policies like One Factor, Two Factor, Bypass or specific groups of allowed users.  Follow [https://www.authelia.com/configuration/security/access-control/](https://www.authelia.com/configuration/security/access-control/)

## Enable Email Notifications from Authelia

Edit the `docker-compose.yml` and comment out the line `- AUTHELIA_NOTIFIER_FILESYSTEM_FILENAME=/config/notifications.txt` and uncomment the lines that start with `- AUTHELIA_NOTIFIER_SMTP`. Fill in the corresponding parts of the .env file with your SMTP server details.

## Go
```
docker compose up -d
```

You should now be able to access ddc at https://caddy.{Domain}.duckdns.org

## Going Forward

Add labels to your containers to configure DDC

``` yaml
  labels:
    - caddy.dynamic.docker.dns=DomainNameIncludingSubdomain
    - caddy.dynamic.docker.target=TargetDnsName:TargetPort
```

The target needs to be hostname, you can't use an IP Address due to the limitations of the SRV protocol.  If the app is on the same network as Caddy you can use it's container name.  You can also setup a public dns record in your DNS service to point to a private IP Address.  
