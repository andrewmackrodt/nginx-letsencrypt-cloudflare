# [andrewmackrodt/nginx-letsencrypt-cloudflare](https://github.com/andrewmackrodt/nginx-letsencrypt-cloudflare)

[docker-compose](https://docs.docker.com/compose/overview/) template for running
a single host ingress server.
- Automatic Let's Encrypt certificate
generation
- Cloudflare DNS modifications
- Service discovery, containers launched globally will work

## Usage

Copy `.env.dist` to `.env` and fill in all fields. At minimum, a free Cloudflare
account is required with DNS configured to run through it.

```sh
# Cloudflare API Token
CF_TOKEN=bf9d3cbb93d0

# Cloudflare E-mail address
CF_EMAIL=admin@mydomain.com

# The CNAME target
CF_TARGET=mydomain.com

# Cloureflare domain name
CF_DOMAIN=mydomain.com

# Cloureflare API Zone ID
CF_ZONE_ID=1234567890

# Let's Encrypt E-mail for Notifications
LETSENCRYPT_EMAIL=admin@mydomain.com
```

Define hosts in `docker-compose.yml`, e.g. to add `jenkins.mydomain.com`, add:

```yml
services:
  ...
  jenkins:
    image: jenkins/jenkins:lts
    environment:
      VIRTUAL_HOST: "jenkins.${CF_DOMAIN}"
      VIRTUAL_PORT: 8080
      LETSENCRYPT_HOST: "jenkins.${CF_DOMAIN}"
      LETSENCRYPT_EMAIL: "${LETSENCRYPT_EMAIL}"
```

Execute docker-compose:

```sh
docker-compose up -d
```

## Advanced Usage

_TODO document defining an explicitly named network so that containers launched
directly or from other compose files are routable._

_Note: this works, it's just not documented yet._
