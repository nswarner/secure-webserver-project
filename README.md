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
