# Basic linux installation steps


### 1. install base system (with/without hdd encryption)
### 2. apt update -y && apt upgrade -y
### 3. install base programs to be able to bootstrap scripts

    apt install curl git git-crypt gpg ssh

if `gpg --card-status` (try also issue command list) complaints about smartcard daemon, like for example:

    gpg: error retrieving URL from card: No SmartCard daemon

you are missing scdaemon (and maybe also: pcscd gnupg2 pcsc-tools) and in that case:

    apt install scdaemon pcscd gnupg2 pcsc-tools

### 4a. in case you have yubikey with ssh: make yubikey work 
   . Add into .bashrc

    export GPG_TTY=$(tty)
    gpg-connect-agent updatestartuptty /bye
    unset SSH_AGENT_PID
    export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)

and restart terminal. Then try: ssh-add -L and you should see public key from yubico. 
If that happens, issue:

    gpg --card-edit 

and issue command fetch. If that does not work, issue

    gpg --keyserver keyserver.ubuntu.com --recv-keys "4AC2 6441 33C8 3D01 1725  D256 EE59 DA2C 892A 0445"

This installs key into gpg, and then you should be able to clone repos using this private key and decrypt its contents

### 4b. in case you don't

    . import id_rsa and id_rsa.pub from older machine OR register new one in githubs from step 5 and 6

    . you will still need to decrypt them, and to do that, you still need to use gpg key which is in your other machine, backup flashdrive or on yubikey.


### 5. clone scripts

    git clone https://github.com/alfonz19/mmucha-linux-conf-and-scripts.git ~/.env-scr

### 6. clone personal notes

    git clone https://github.com/alfonz19/notes.git ~/adocNotes

### 7. go into both repos from step 5 and 6, do `git-crypt unlock` and continue with _install.adoc file 
