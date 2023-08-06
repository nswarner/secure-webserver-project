# Tasks - Configuring Ansible

## Assumptions

1. You have a running Debian Linux docker container named `debian_linux`
2. You are either SSH'd into the container or have `docker exec` into the container
3. The container mounts a local `.\srv` directory to the container's `/srv` directory
  * `/srv/ansible` exists and is empty (or content can be overwritten)
4. SSH Keys and GPG Keys have been setup in GitHub

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
4. Ensure that you Git `add`, `commit`, and `push`, all files to your GitHub repository
