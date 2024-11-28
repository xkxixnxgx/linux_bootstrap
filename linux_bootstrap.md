##### ! For example username is `harry`.  Set your system username in all places. 

```bash
su -
usermod -aG sudo harry
nano /etc/sudoers
```
##### add `harry` user to the file end to get this text block 
```/etc/sudoers
# User privilege specification
root ALL=(ALL:ALL) ALL
harry ALL=(ALL) ALL
```

```bash
su harry
```

# zsh
```bash
sudo apt-get install zsh
zsh --version

sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

##### Input command with user password for sudo and relogin to system
```bash
chsh -s $(which zsh)
```

##### download plugins
```bash
cd ~/.oh-my-zsh/custom/plugins
git clone https://github.com/zsh-users/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting
```
##### config
```bash
nano ~/.zshrc
```

```txt
ZSH_THEME="robbyrussell"
plugins=(
  git
  docker
  docker-compose
  tmux
  python
  poetry
  zsh-autosuggestions
  zsh-syntax-highlighting
)
```
##### add to `~/.zshrc`
```~/.zshrc
export PATH="$HOME/.local/bin:$PATH"
```

```bash
source ~/.zshrc
```
# Terminal Alacritty

### Install Rust
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
nano ~/.zshrc
```
##### Add string to your config file
```./zshrc
export PATH=$PATH:~/.cargo/bin/
```
##### Check cargo
```bash
cargo --version
```
##### Install Alacritty
```bash
apt install cmake g++ pkg-config libfreetype6-dev libfontconfig1-dev libxcb-xfixes0-dev libxkbcommon-dev python3

git clone https://github.com/alacritty/alacritty.git && cd alacritty

rustup override set stable
rustup update stable

cargo build --release

sudo cp target/release/alacritty /usr/local/bin # or anywhere else in $PATH
sudo cp extra/logo/alacritty-term.svg /usr/share/pixmaps/Alacritty.svg
sudo desktop-file-install extra/linux/Alacritty.desktop
sudo update-desktop-database

mkdir -p ${ZDOTDIR:-~}/.zsh_functions
echo 'fpath+=${ZDOTDIR:-~}/.zsh_functions' >> ${ZDOTDIR:-~}/.zshrc
cp extra/completions/_alacritty ${ZDOTDIR:-~}/.zsh_functions/_alacritty
```
# wireguard
[wireguard link](https://help.keenetic.com/hc/ru/articles/360010511219-%D0%9F%D0%BE%D0%B4%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D0%B5-%D0%BF%D0%BE-%D0%BF%D1%80%D0%BE%D1%82%D0%BE%D0%BA%D0%BE%D0%BB%D1%83-WireGuard-VPN-%D0%B8%D0%B7-Linux)
```bash
sudo apt install wireguard
sudo apt install resolvconf
sudo ip link add dev wg0 type wireguard
sudo chown -R harry /etc/wireguard/
cd /etc/wireguard
```
### copy  settings to `wg-client.conf` file
```bash
sudo nano wg-client.conf
sudo systemctl start wg-quick@wg-client.service
sudo systemctl enable wg-quick@wg-client.service
sudo chown -R root /etc/wireguard/
```
### check ping to server in vpn network
```bash
ping 10.90.1.0
sudo wg show
```
### create autorun with system starting
```bash
sudo systemctl enable wg-quick@wg-client.service
```
### stop connection
```bash
sudo systemctl stop wg-quick@wg-client.service
```
### start connection
```bash
sudo systemctl start wg-quick@wg-client.service
```
### check connection status
```bash
sudo systemctl status wg-quick@wg-client.service
```
### turn off autorun with system starting
```bash
sudo systemctl disable wg-quick@wg-client.service
```
# docker
```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
newgrp docker
sudo setfacl --modify user:harry:rw /var/run/docker.sock
sudo docker run hello-world
docker --version
```
# docker-compose

```bash
sudo curl -SL https://github.com/docker/compose/releases/download/v2.30.3/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
sudo usermod -aG docker harry
sudo chgrp harry /usr/local/bin/docker-compose
sudo chmod 750 /usr/local/bin/docker-compose
docker-compose --version
```
# ssh key for gitlab
```bash
cd ~/.ssh
rm gitlab
rm gitlab.pub
ssh-keygen
-> Enter file in which to save the key (/home/harry/.ssh/id_rsa): gitlab
```
# pyenv
```bash
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
nano ~/.zshrc
```
##### добавляем следующий код в конец файла `~/.zshrc`
```~/.zshrc
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```
##### reload
```bash
source ~/.zshrc
```
##### install packeage dependancies
```bash
sudo apt install libedit-dev build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev curl \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```
##### check pyenv
```bash
pyenv install 3.11.2
cd ~/cr/backend2
pyenv virtualenv 3.11.2 venv_backend2
pyenv local venv_backend2
```
# poetry
```bash
curl -sSL https://install.python-poetry.org | python3 -
source ~/.zshrc
poetry --version
```
# obsidian
##### [obsidian link](https://github.com/obsidianmd/obsidian-releases/releases/download/v1.7.7/obsidian_1.7.7_amd64.deb)
```bash
sudo dpkg -i obsidian_1.7.7_amd64.deb
```
# tmux
```bash
sudo apt install tmux
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
nano ~/.tmux.conf
```

```txt
set -g default-terminal "screen-256color"

# General prefix
set -g prefix C-a

# sort by name
bind s choose-tree -sZ -O name

# index changing
set -g base-index 1
setw -g pane-base-index 1

# key reset
unbind %
bind | split-window -h 

unbind '"'
bind - split-window -v

unbind r
bind r source-file ~/.tmux.conf

bind -r j resize-pane -D 5
bind -r k resize-pane -U 5
bind -r l resize-pane -R 5
bind -r h resize-pane -L 5

bind -r m resize-pane -Z

set -g mouse on

set-window-option -g mode-keys vi

bind-key -T copy-mode-vi 'v' send -X begin-selection 
bind-key -T copy-mode-vi 'y' send -X copy-selection 

unbind -T copy-mode-vi MouseDragEnd1Pane

# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'christoomey/vim-tmux-navigator'
set -g @plugin 'jimeh/tmux-themepack'
set -g @plugin 'tmux-plugins/tmux-resurrect' 
set -g @plugin 'tmux-plugins/tmux-continuum'
set -g @plugin 'tmux-plugins/tmux-sessionist'

set -g @themepack 'powerline/default/purple'

set -g @resurrect-capture-pane-contents 'on'
set -g @continuum-restore 'on'

# Initialize TMUX plugin manager (keep this line at the very 
run '~/.tmux/plugins/tpm/tpm'
```

```bash
tmux source ~/.tmux.conf
```

```bash
tmux
```

```tmux
Press `Ctl+a` + I (capital i, as in **I**nstall) to fetch the plugin.
```
# NvChad - Nvim IDE
### Font
```bash
mkdir ~/.local/share/fonts

#wget -P ~/.local/share/fonts https://www.gust.org.pl/projects/e-foundry/tex-#gyre/cursor/qcr2.004bas.zip \  
#&& cd ~/.local/share/fonts \  
#&& unzip qcr2.004bas.zip \  
#&& rm qcr2.004bas.zip \  
#&& fc-cache -fv

wget -P ~/.local/share/fonts https://github.com/ryanoasis/nerd-fonts/releases/download/v3.3.0/Overpass.zip
cd ~/.local/share/fonts
unzip Overpass.zip
rm Overpass.zip
fc-cache -fv

wget -P ~/.local/share/fonts https://github.com/ryanoasis/nerd-fonts/releases/download/v3.3.0/NerdFontsSymbolsOnly.zip \  
cd ~/.local/share/fonts \  
unzip NerdFontsSymbolsOnly.zip \  
rm NerdFontsSymbolsOnly.zip \  
fc-cache -fv

```
### Download Nvim 0.10.2 
```bash
wget -P ~/ https://github.com/neovim/neovim-releases/releases/download/v0.10.2/nvim-linux64.tar.gz

tar xzvf nvim-linux64.tar.gz && rm ~/nvim-linux64.tar.gz
nano ~/.zshrc
```

```txt
# nvim
export PATH="$PATH:$HOME/nvim-linux64/bin"
```

```bash
source ~/.zshrc
nvim
# for exit input :q + Enter
```

##### Download  ripgrep 
```
curl -LO https://github.com/BurntSushi/ripgrep/releases/download/14.1.0/ripgrep_14.1.0-1_amd64.deb
sudo dpkg -i ripgrep_14.1.0-1_amd64.deb
rm ripgrep_14.1.0-1_amd64.deb
rg --version
```
##### npm
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
source ~/.zshrc
nvm install 22
node -v
npm -v
```
##### Download NvChad
```bash
git clone https://github.com/NvChad/starter ~/.config/nvim && nvim
sudo apt install -y python3-venv
sudo apt install liblua5.4-dev
```
##### Install luarocks lib
```bash
sudo apt install liblua5.4-dev

cd ~/.config
wget https://luarocks.org/releases/luarocks-3.11.1.tar.gz
tar zxpf luarocks-3.11.1.tar.gz
rm luarocks-3.11.1.tar.gz
cd luarocks-3.11.1
./configure && make && sudo make install
sudo luarocks install luasocket
```
##### Install 
```nvim
:MasonInstallAll
:MasonInstall rust-analyzer --force
:q
:q
```

##### NvChad commands
```bash
rm -rf ~/.config/nvim/.git
cd ~/.config/nvim
git init
git branch -m main
git config --global init.defaultBranch main
```
# Git settings
##### Set local email and username for directory but not for global git

```bash
git config --global user.name "Harry Potter"
git config --global user.email "harry-potter@gmail.com"

cd /home/harry/your-project
git config user.name "Jhon Black"
git config user.email "jhon-black@gmail.com"

```
# Customize GNOM
```bash
sudo apt install -y --no-install-recommends chrome-gnome-shell gnome-shell-extension-dashtodock gnome-shell-extension-manager gnome-shell-extensions gnome-shell-extensions-extra gnome-tweaks conky-all mpd lua5.4 jq imagemagick
```

# vscode
##### [vscode](https://code.visualstudio.com/docs/setup/linux)
```bash
sudo apt install ./code_1.95.3-1731513102_amd64.deb
```
# telegram

##### [telegram desktop](https://desktop.telegram.org/)
```bash
wget -P ~/.config/ "https://telegram.org/dl/desktop/linux"
tar -xvf ~/.config/tsetup.5.8.3.tar.xz
rm ~/.config/tsetup.5.8.3.tar.xz
```

if you can not find icon in app bar
```bash
wget -O ~/.config/Telegram/telegram.svg "https://commons.wikimedia.org/wiki/File:Telegram_logo.svg"
sudo nano /usr/share/applications/telegram.desktop
```

```telegram.desktop
[Desktop Entry]
Version=5.8.3
Type=Application
Name=Telegram
Exec=/home/harry/.config/Telegram/Telegram
Icon=/home/harry/.config/Telegram/telegram.svg
Terminal=false
Categories=Chat;
```

```bash
update-desktop-database ~/.local/share/applications
```