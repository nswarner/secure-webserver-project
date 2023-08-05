# secure-webserver-project

# Tasks:

## Prework

* Install "Docker Desktop" on your local computer
* Install "Notepad++" on your local computer
Expand
message.txt
6 KB
﻿
# Tasks:

## Prework

* Install "Docker Desktop" on your local computer
* Install "Notepad++" on your local computer
* Pin both to your Taskbar
* Pin "Powershell" to your Taskbar
* Log into "GitHub"

## Configuring for GitHub connectivity, setting up local environment

1. In Powershell, c&p `docker run --name debian_linux --rm debian:latest`
  * After the container is running, exec into the container, `docker exec -it debian_linux /bin/bash`
2. Generate an SSH Keypair: `ssh-keygen -t ed25519 -C '<email>'
3. Copy the `.pub` public key
4. In GitHub, in your Profile, under Security/SSH, enter the public SSH Key
5. In GitHub, create a new repository
6. In the Debian container (step #1), clone the new repository via SSH
  * `git clone <>`
7. `cd <directory>` of the cloned repository
8. Create local directories `./web`, `./certs`, and `./ansible`
9. Create a placeholder file in each directory
  * `touch ./web/placeholder`
  * `touch ./certs/placeholder`
  * `touch ./ansible/placeholder`
10. Git `add`, `commit`, and `push` the contents of `./web`, `./certs`, and `./ansible` directories
11. Verify these have been pushed to the remote GitHub repository

## Setting up a website

1. Please create a simple three page website, with all files saved into the `./web` directory
  - The index page will have a header, footer, left navigation, with each component having a different thematic style, store this file into `./web/index.html`
  - The about us page will header a header, footer, left navigation, with each component having a different thematic style, store this file into `./web/about.html`
  - The content page will header a header, footer, left navigation, with each component having a different thematic style, store this file into `./web/content.html`
  - Each page should share an overall CSS theme but have some unique styles
  - The navigation on each page should reference the `index.html`, `about.html`, and `content.html` pages
  - The content page should have placeholders for images in the body of the page
    - Replace the image placeholders with actual images saved to `./web/images/<imagename[].png>`
2. Ensure that you Git `add`, `commit`, and `push, all files to your GitHub repository

## Setting up the local development environment

1. Please create a local docker image configured to:
  a. Add a new user (your username) with your public key stored in your home directory, `~/.ssh/authorized_users`
  b. Configure to allow SSH with public keys, disable root login, and disable password login
  c. Name and tag the image `base-debian-linux:v1.0.0`
  d. Enable sudo access for your new user
2. Please locally start three `debian:latest` containers using docker-compose and your base image, `base-debian-linux:v1.0.0`
  a. All containers expose port 22 for SSH access
  b. All containers are configured to always restart
  c. Nginx (website) server:
    - Expose port 8080
	- Mount a local directory (`./web`) to remote container directory (`/srv`)
	- Mount a local directory (`./certs`) to remote container directory (`/certs`)
  d. Root CA server: 
    - Mount a local directory (`./certs`) to remote container directory (`/certs`)
  e. Ansible server:
    - Mount local directory (`./playbooks`) to remote directory (`/playbooks`)
3. Name each container `nginx`, `rootca`, and `ansible` respectively
4. Ensure that you Git `add`, `commit`, and `push, the docker-compose file to your GitHub repository

## Setup the layout for Ansible

1. Please create the layout for Ansible to use Playbooks, Roles, Variables, Vaults, and Inventories in the `./ansible` directory
  a. Please create a placeholder in each of the directories if a file does not exist
  b. The `roles` directory should have two roles, `nginx_setup` and `root_ca_setup`
  c. Each of these roles should have `tasks`, `files`, `templates`, and `handlers` as subdirectories
2. Please create an Ansible playbook on your local computer, located in `./ansible/playbooks` directory, which has three roles; 
  a. One sets up Nginx on port 8080 with a specified TLS certificate stored in `/cert` (this is a placeholder)
    - You can either configure Nginx to set its web directory to `/srv` or
    - You can copy the `/srv` directory website files to the Nginx web directory
	- This will only run against the `nginx` host
  b. One creates a root certificate authority certificate (root CA), an intermediate CA certificate, and an end-entity certificate
	- The end-entity certificate will be for `nginx.internaldns.com`
	- This will only run against the `rootca` host
  c. One which determines the host operating system, hostname, IP, and list of installed packages
    - This will display all of this information in a debug message
	- This will run against all hosts
3. Please create an inventory file with the following layout:
  a. It will have a group for `webservers` with the `nginx` container in it
  b. It will have a group for `certificates` with the `rootca` container in it
  c. You will find the `nginx` container IP and create an entry for `nginx <ip>`
  d. You will find the `rootca` container IP and create an entry for `rootca <ip>`
4. Ensure that you Git `add`, `commit`, and `push, all files to your GitHub repository
message.txt
6 KB
