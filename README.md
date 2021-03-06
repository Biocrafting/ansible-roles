# ansible-roles
Some ansible roles to manage our server/services.

Feel free to use/modify this roles for your needs.
Please bear this roles are intended for use with our environment. They are not generalized in any way.


## letsencrypt-host-prepare-nginx
This role prepares a server with nginx for our [Let's Encrypt renewal strategy](https://4g.noobadmin.de/blog/2016/08/11/letsencrypt-catchall-acme-response/).

It is  tested on Debian Jessie/ CentOS 7 hosts, but should also work on other distros aslong they provide the package "sudo" and uses systemd for service management.

The role installs sudo, creates a user with the homedirectory in /etc/ssl, adds the public key found in `files/public_key` to the authorized_keys (with some limitations, like only allowed from the address defined in `{{ le.host }}`) and configures sudo, so the user can reload the `nginx.service`.

You can set two variables:

| Variable   | Use   | 
|------|---|
| le.user | The user which is used by ansible in the letsencrypt-renew-* roles. |
| le.host   | host/network (with CIDR notation) which will execute the ansible roles. |
## letsencrypt-host-renew-nginx

This role deploys the certificate/key combo (we are using [acme.sh](https://github.com/Neilpang/acme.sh) for this job) on the servers and generate a nginx config based on that.
Your vhosts have to include `/etc/ssl/{{ le.user }}/letsencrypt/nginx-ssl.conf`.

| Variable   | Use   | 
|------|---|
| le.cert   | location of the certificate file which should be deployed on the host  |
| le.key   | location of the key file which should be deployed on the host  |
