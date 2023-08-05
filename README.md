# secure-webserver-project

# Tasks:

## Prework

* Install "Docker Desktop" on your local computer
* Install "Notepad++" on your local computer
* Pin both to your Taskbar
* Pin "Powershell" to your Taskbar
* Log into "GitHub"

## Configuring for GitHub connectivity, setting up local environment

**SECURITY:** never share your private key with anyone

1. In Powershell,
  * Run `mkdir root`, `mkdir ssh`
  * Run `docker run --name debian_linux --rm -itd -v .\srv:/srv -v .\ssh:/root/.ssh debian:latest`
  * After the container is running, exec into the container, `docker exec -it debian_linux /bin/bash`
2. Run `apt update && apt install openssl openssh-client dos2unix procps`
3. Generate an SSH Keypair: `ssh-keygen -t ed25519 -C '<email>'`
  * `Enter file in which to save the key (/root/.ssh/id_ed25519):` <-- press enter, leave it default
    ** If it asks you if you want to overwrite, choose `n` and press enter
  * `Enter passphrase (empty for no passphrase):` <-- enter a password (secret)
  * `Enter same passphrase again:` <-- enter the same password (secret)
  * **Your public key has been saved in /root/.ssh/id_ed25519.pub**
  * **Your identification has been saved in /root/.ssh/id_ed25519** (private key)
  * Run `cat ~/.ssh/id_ed25519.pub` to see your **public** key (`.pub`)
4. Copy the `.pub` public key
5. In GitHub, in your Profile, under Security/SSH, enter the public SSH Key
6. In GitHub, create a new repository
7. In the Debian container (step #1), clone the new repository via SSH
  * `cd /srv && git clone <ssh-url>`
8. `cd <directory>` of the cloned repository (`ls` to see directories)
9. Create local directories `./web`, `./certs`, and `./ansible`
10. Create a placeholder file in each directory
  * `touch ./web/placeholder`
  * `touch ./certs/placeholder`
  * `touch ./ansible/placeholder`
11. Git `add`, `commit`, and `push` the contents of `./web`, `./certs`, and `./ansible` directories
12. Verify these have been pushed to the remote GitHub repository

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
  - Add a new user (your username) with your public key stored in your home directory, `~/.ssh/authorized_users`
  - Configure to allow SSH with public keys, disable root login, and disable password login
  - Name and tag the image `base-debian-linux:v1.0.0`
  - Enable sudo access for your new user
2. Please locally start three `debian:latest` containers using docker-compose and your base image, `base-debian-linux:v1.0.0`
  - All containers expose port 22 for SSH access
  - All containers are configured to always restart
  - Nginx (website) server:
    - Expose port 8080
	- Mount a local directory (`./web`) to remote container directory (`/srv`)
	- Mount a local directory (`./certs`) to remote container directory (`/certs`)
  - Root CA server: 
    - Mount a local directory (`./certs`) to remote container directory (`/certs`)
  - Ansible server:
    - Mount local directory (`./playbooks`) to remote directory (`/playbooks`)
3. Name each container `nginx`, `rootca`, and `ansible` respectively
4. Ensure that you Git `add`, `commit`, and `push, the docker-compose file to your GitHub repository

## Setup the layout for Ansible

1. Please create the layout for Ansible to use Playbooks, Roles, Variables, Vaults, and Inventories in the `./ansible` directory
  - Please create a placeholder in each of the directories if a file does not exist
  - The `roles` directory should have two roles, `nginx_setup` and `root_ca_setup`
  - Each of these roles should have `tasks`, `files`, `templates`, and `handlers` as subdirectories
2. Please create an Ansible playbook on your local computer, located in `./ansible/playbooks` directory, which has three roles; 
  - One sets up Nginx on port 8080 with a specified TLS certificate stored in `/cert` (this is a placeholder)
    - You can either configure Nginx to set its web directory to `/srv` or
    - You can copy the `/srv` directory website files to the Nginx web directory
	- This will only run against the `nginx` host
  - One creates a root certificate authority certificate (root CA), an intermediate CA certificate, and an end-entity certificate
	- The end-entity certificate will be for `nginx.internaldns.com`
	- This will only run against the `rootca` host
  - One which determines the host operating system, hostname, IP, and list of installed packages
    - This will display all of this information in a debug message
	- This will run against all hosts
3. Please create an inventory file with the following layout:
  - It will have a group for `webservers` with the `nginx` container in it
  - It will have a group for `certificates` with the `rootca` container in it
  - You will find the `nginx` container IP and create an entry for `nginx <ip>`
  - You will find the `rootca` container IP and create an entry for `rootca <ip>`
4. Ensure that you Git `add`, `commit`, and `push, all files to your GitHub repository
message.txt
