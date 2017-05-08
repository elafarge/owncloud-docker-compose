Docker-Compose deployment for OwnCloud
======================================

![WTFPL](http://www.wtfpl.net/wp-content/uploads/2012/12/wtfpl-badge-4.png)

This simple `docker-compose.yaml` setup aims at getting an OwnCloud/Postgres
instance up & running in seconds on any box (would it be a server in your
bedroom, a dedicated server in a datacenter or a VM somewhere in the cloud).

Requirements
------------

You'll need... just `docker` and his mate `docker-compose` :)

This setup is intended to run behind a [traefik reverse proxy](https://traefik.io/).
If you don't need this however, you can simply remove traefik labels from the
OwnCloud container in the Yaml manifest and expose port 80 as follows:
```yaml
    ports:
    - 80:80
```
**Note that not using TLS on your personnal might be a really bad idea :)**

### Getting a TLS-enabled proxy in one command (Traefik + Let's Encrypt = <3)

@hkaj and I crafted [yet another Docker-Compose setup](https://github.com/hkaj/reverse_proxy) that gets a Traefik reverse
proxy running (and fetching all TLS certs in needs automatically using Let's
Encrypt) in about one single command. Use it without moderation :)

Setup
-----

Generate a password to secure the connection between your OwnCloud container
and its postgresql one. If you don't feel inspired today, you can generate a
random 16-character password issuing this command:
```shell
date +%s | sha256sum | base64 | head -c 16 ; echo
```

And finally, clone this repo, `cd` into it and just run (don't forget to
replace `DOMAIN_NAME` and `POSTGRES_VALUES` with a domain name that points to
your box and a more secure passphrase):
```shell
DOMAIN_NAME=example.com POSTGRES_PASSWORD=yourpassword docker-compose up -d
```

You can now visit `https://cloud.example.com`. On the setup page, choose
`PostgreSQL` for the database type. Enter `owncloud` in the `Database user`
**and** `Database name` fields, replace `localhost` with `postgres` and
`Database password` with the password you entered in the above command.
