# GitHub signed commits by GPG Key

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
