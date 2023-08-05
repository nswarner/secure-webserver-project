# Tasks - Create running containers to run the website and root CA server

## Assumptions

1. You have a running Debian Linux docker container named `debian_linux`
2. You are either SSH'd into the container or have `docker exec` into the container
3. The container mounts a local `.\srv` directory to the container's `/srv` directory
  * `/srv/web` exists and is empty (or content can be overwritten)
4. SSH Keys and GPG Keys have been setup in GitHub

## Setting up the local environment

In a new Powershell window with Notepad++,

1. Please create a local docker image configured to:
  * Add a new user (your username) with your public key stored in your home directory, `~/.ssh/authorized_users`
  * Configure to allow SSH with public keys, disable root login, and disable password login
  * Name and tag the image `base-debian-linux:v1.0.0`
  * Enable sudo access for your new user
2. Please locally start three `debian:latest` containers using docker-compose and your base image, `base-debian-linux:v1.0.0`
  * All containers expose port 22 for SSH access
  * All containers are configured to always restart
  * Nginx (website) server:
    * Expose port 8080
      * Mount a local directory (`./web`) to remote container directory (`/srv`)
      * Mount a local directory (`./certs`) to remote container directory (`/certs`)
  * Root CA server: 
    * Mount a local directory (`./certs`) to remote container directory (`/certs`)
  * Ansible server:
    * Mount local directory (`./ansible`) to remote directory (`/ansible`)
3. Name each container `nginx`, `rootca`, and `ansible` respectively
4. Ensure that you Git `add`, `commit`, and `push`, the docker-compose file to your GitHub repository
