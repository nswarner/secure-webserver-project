# secure-webserver-project

# Tasks - Configure the local development environment:

## Prework

* Install "Docker Desktop" on your local computer
* Install "Notepad++" on your local computer
* Pin both to your Taskbar
* Pin "Powershell" to your Taskbar
* Log into "GitHub"

## Configuring for GitHub connectivity, setting up local environment

### GitHub Clone by SSH
**SECURITY:** never share your private key with anyone

1. In Powershell,
  * Run `mkdir root`, `mkdir root/.ssh`, `mkdir /root/.gnupg`
  * Run `docker run --name debian_linux --rm -itd -v .\srv:/srv -v .\root:/root debian:latest`
  * After the container is running, exec into the container, `docker exec -it debian_linux /bin/bash`
2. Run `apt update && apt install openssl openssh-client dos2unix procps gpg git`
3. Generate an SSH Keypair: `ssh-keygen -t ed25519 -C '<email>'`
  * `Enter file in which to save the key (/root/.ssh/id_ed25519):` <-- press enter, leave it default
    * If it asks you if you want to overwrite, choose `n` and press enter
  * `Enter passphrase (empty for no passphrase):` <-- enter a password (secret)
  * `Enter same passphrase again:` <-- enter the same password (secret)
  * **Your public key has been saved in /root/.ssh/id_ed25519.pub**
  * **Your identification has been saved in /root/.ssh/id_ed25519** (private key)
  * Run `cat ~/.ssh/id_ed25519.pub` to see your **public** key (`.pub`)
4. Copy the `.pub` public key
5. In GitHub, in your Profile, under Security/SSH, enter the public SSH Key

### GitHub signed commits by GPG Key

Ref: https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key

Ref: https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account

1. In the `debian_linux` container,
  * `gpg --full-generate-key` - generate a GPG Key
  * Selection 1: `RSA and RSA (default)`
  * Selection 2: `4096`
  * Selection 3: `0`
  * Selection 4: `y`
  * Selection 5: `<your real name>`
  * Selection 6: `<your real email address>`
  * Selection 7: `<leave comment blank and press enter>`
  * Selection 8: `O` (Ok)
  * Selection 9: `<enter a password>`
  * Selection 10: `<repeat the password>`
2. Run `gpg --list-secret-keys --keyid-format=long` to see the key you've created
  * From the line that starts with `sec`, e.g. `sec   4096R/3AA5C34371567BD2`, copy the value after `4096R/`, i.e. copy `3AA5C34371567BD2`
3. Run `gpg --armor --export 3AA5C34371567BD2`, but replace `3AA5C34371567BD2` with the value you copied
4. Copy your GPG key, beginning with `-----BEGIN PGP PUBLIC KEY BLOCK-----` and ending with `-----END PGP PUBLIC KEY BLOCK-----`
  * Make sure to include the `-----BEGIN...` and `-----END...` lines
5. In GitHub, add the copied Public GPG Key to your account
6. Configure `git` to sign your commits, `git config --global --unset gpg.format`
7. Configure `git` with your signing key, `git config --global user.signingkey 3AA5C34371567BD2`
  * Replace `3AA5C34371567BD2` with the value you copied in #2
8. Configure `git` to sign all commits by default, `git config --global commit.gpgsign true`

Note: Alternatively, you can sign with a pre-existing SSH Key, but it's good to follow the workflow for GPG just for knowledge, https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key#telling-git-about-your-ssh-key

### Setup a new GitHub repository

1. In GitHub, create a new public repository
  * Click the big green "Code" button in the repository, choose "Local" and "SSH" to get the `clone` URL
2. In the Debian container, clone the new repository via SSH
  * `cd /srv && git clone <ssh-url>`
3. `cd <directory>` of the cloned repository (`ls` to see directories)
4. Create local directories `./web`, `./certs`, and `./ansible`
5. Create a placeholder file in each directory
  * `touch ./web/placeholder`
  * `touch ./certs/placeholder`
  * `touch ./ansible/placeholder`
6. Git `add`, `commit`, and `push` the contents of `./web`, `./certs`, and `./ansible` directories
7. Verify these have been pushed to the remote GitHub repository

# Tasks - Building a website

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

# Tasks - Configuring Ansible

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

# Tasks - Create running containers to run the website and root CA server

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
4. Ensure that you Git `add`, `commit`, and `push, the docker-compose file to your GitHub repository
