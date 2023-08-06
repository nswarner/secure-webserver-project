# Setup a new GitHub repository

1. In GitHub, create a new public repository
  * Click the big green "Code" button in the repository, choose "Local" and "SSH" to get the `clone` URL
2. In the Debian container, clone the new repository via SSH
  * `cd /srv && git clone <ssh-url>`
3. `cd <directory>` of the cloned repository (`ls` to see directories)
4. Create local directories `mkdir ./web`, `mkdir ./certs`, and `mkdir ./ansible`
5. Create a placeholder file in each directory
  * `touch ./web/placeholder`
  * `touch ./certs/placeholder`
  * `touch ./ansible/placeholder`
6. Git `add`, `commit`, and `push` the contents of `./web`, `./certs`, and `./ansible` directories
7. Verify these have been pushed to the remote GitHub repository
