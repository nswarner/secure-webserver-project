# GitHub Clone by SSH

**SECURITY:** never share your private key with anyone

1. In Powershell,
  * Run `mkdir root`, `mkdir root/.ssh`, `mkdir root/.gnupg`, `mkdir srv`
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
