# 3. Android Lab Setup

## Kali Linux





### MAPT Course Setup

```bash
cd
mkdir repo
cd repo
git clone https://github.com/Dewalt-arch/pimpmykali
cd pimpmykali
sudo ./pimpmykali.sh
# Select Option A for MAPT course tools

sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y

sudo apt install adb apktool default-jdk jadx

# Android Studio #
sudo dpkg --add-architecture i386

sudo apt update && sudo apt install -y libc6:i386 libncurses5:i386 libstdc++6:i386 lib32z1 libbz2-1.0:i386

sudo wget https://redirector.gvt1.com/edgedl/android/studio/ide-zips/2023.1.1.26/android-studio-2023.1.1.26-linux.tar.gz -O /tmp/android-studio.tar.gz

sudo tar xvfz /tmp/android-studio.tar.gz -C /opt
sudo chmod +x /opt/android-studio/bin/*.sh
sudo rm -f /tmp/android-studio.tar.gz

## Run with:
cd /opt/android-studio/bin
./studio.sh
## Proceed with the Setup Wizard selecting all the available components to install
## Open a New project and go to Tools > Create Command-line Launcher & Create Desktop Entry

# Genymotion #
sudo apt install -y virtualbox adb
sudo wget https://dl.genymotion.com/releases/genymotion-3.5.1/genymotion-3.5.1-linux_x64.bin -O /tmp/genymotion.bin

sudo chmod +x genymotion.bin
sudo ./genymotion.bin

cd /opt/genymobile/genymotion
./genymotion
```

### Additional Personal Setup

```bash
# my script

local packages=(
        # Add software here
        aapt adb android-sdk-platform-tools apksigner apktool apt-transport-https aptitude bettercap 
        btop cifs-utils curl dbeaver duf exa flameshot firefox-esr flatpak fonts-firacode
        fonts-noto-color-emoji htop ideviceinstaller imagemagick jadx kate
        keepassxc libimobiledevice-utils libimobiledevice6 libusbmuxd-tools locate lolcat net-tools
        npm openssl python3 python3-pip python3-venv pipx qpdfview sqlite3 sqlitebrowser
        sshpass terminator tree ugrep virt-manager vlc wget wkhtmltopdf zaproxy zipalign
    )
sudo apt update && sudo apt install -y -o Debug::pkgProblemResolver=yes "${packages[@]}"
```



------

