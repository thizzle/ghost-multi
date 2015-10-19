# Ghost Multi-Blog Infrastructure

Use [Ansible](http://www.ansible.com/) to setup infrastructure for hosting multiple Ghost blog sites.

This system relies on several projects, all co-ordinated by [Docker](https://www.docker.com/), the container engine for running each service:
* [nginx](http://nginx.org/) as the web server and reverse proxy,
* [docker-gen](https://github.com/jwilder/docker-gen) keeps the nginx configuration file in sync with the containers running each blog,
* [ghost](https://ghost.org/) is the blog platform of choice.

## Usage

You'll need `ansible` installed, and available on your `$PATH`.
See the [Ansible Installation Guide](http://docs.ansible.com/ansible/intro_installation.html) if `ansible-playbook` is not yet on your `$PATH`.

### Setup Hostnames

Create file `group_vars/all.yml` containing key "blogs" whose value is an array of string hostnames.
These hostnames will be configured in nginx to proxy to the corresponding blog.

```yaml
# group_vars/all.yml
---

blogs:
  - blog1.example.com
  - blog2.example.net
```

### Setup Inventory

Create file `inventories/default` describing the servers that Ansible will provision.

```
default ansible_ssh_host=<ip_addr>
```

### Run Ansible

Apply the playbook `site.yml`.

```
$ ansible-playbook -i inventories/default site.yml
```

## Notes

This playbook should work against any server running a RHEL distribution.
It has been tested against CentOS 7.x.

## TODO

* Setup automated backup (snapshots locally, possibly configurable to S3)
* Support sending e-mails via local mail agent or SMTP
