# Ubuntu: Avoid typing passphrase on 'ssh'

Here:
<https://unix.stackexchange.com/questions/12195/how-to-avoid-being-asked-passphrase-each-time-i-push-to-bitbucket>

Two solutions:

## Using ssh-agent

    eval $(ssh-agent)
    ssh-add

## Using keychain (debian/ubuntu)

    apt-get install keychain

## Using 'ForwardAgent yes' in .ssh/config

Here: <https://developer.github.com/v3/guides/using-ssh-agent-forwarding>

If it doesn't work at first, check that the key ~/.ssh/id_rsa is added to
ssh-agent, i.e., ssh-add has been run. To avoid re-typing 'ssh-add' and
password on every startup, run:

    ssh-add -K id_rsa
