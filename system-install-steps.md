# Basic linux installation steps


### 1. install base system (with/without hdd encryption)
### 2. apt update -y && apt upgrade -y
### 3. install base programs to be able to bootstrap scripts

    apt install curl git git-crypt gpg 

if `gpg --card-status` (try also issue command list) complaints about smartcard daemon, like for example:

    gpg: error retrieving URL from card: No SmartCard daemon

you are missing scdaemon (and maybe also: pcscd gnupg2 pcsc-tools) and in that case:

    apt install scdaemon pcscd gnupg2 pcsc-tools

### 4. make yubikey work 
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


### 5. git clone git@github.com:alfonz19/mmucha-linux-conf-and-scripts.git /home/mmucha/.env-scr
### 6. git clone git@github.com:alfonz19/notes.git /home/mmucha/adocNotes

