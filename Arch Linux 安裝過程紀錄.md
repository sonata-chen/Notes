# **Arch Linux 安裝過程紀錄**


### 進入開機媒體

- 鍵盤配置
  * 保留預設值即可 (US)

- 連線到網際網路

  * 使用 `ip a` 檢查網卡是否正常運作
  * 使用 `wifi-menu` 連上無線網路
  * 使用 `dhcpcd` 連上有線網路 (動態 ip)
  * 使用 `ping` 確認網路連線

- 編輯 `/etc/pacman.d/mirrorlist`，新增海洋大學映像站
  * `Server = http://shadow.ind.ntou.edu.tw/archlinux/$repo/os/$arch`

- 安裝其他套件
  * `networkmanager`
  * `grub`
  * `man`


### After installation (系統設定)
- 網路連線
    - `nmtui`
- 使用者管理
    - `pacman -S sudo`
    - 新增 billson 使用者
    - 將 billson 加入 **wheel** 群組
    - 編輯 `/etc/sudoers`，執行 `sudo EDITOR=vim visudo`
- 安裝桌面環境 `pacman -S xfce4 lightdm lightdm-webkit2-greeter`
    - 啟用 lightdm `systemctl enable lightdm`
    - 下次開機時，直接進入圖形化界面 `systemctl set-default graphical.target`
    - 編輯 `/etc/lightdm/lightdm.conf`，設定 `greeter` 
        
        ```
        [Seat:*]
        ...
        ...
        #greeter-session=example-gtk-gnome
        greeter-session=lightdm-webkit2-greeter
        #greeter-hide-users=false        
        ...
        [XDMCPServer]
        ```
- 在使用者家目錄下產生 Downloads Documents Desktop Music 等八個常用目錄 ([XDG user directories](https://wiki.archlinux.org/index.php/XDG_user_directories))
    - `pacman -S xdg-user-dirs`
    - 執行 `xdg-user-dirs-update`
- 安裝中文輸入法和其他字型
    - `pacman -S noto-fonts noto-fonts-cjk noto-fonts-emoji terminus-font`
    - `pacman -S fcitx-im fcitx-chewing fcitx-configtool`
    - 編輯 `~/.xprofile`
        
        ``` bash
        export XMODIFIERS=@im=fcitx
        export GTK_IM_MODULE=fcitx
        export QT_IM_MODULE=fcitx
        fcitx&
        ```        
- 安裝網頁瀏覽器 **Firefox** `pacman -S firefox` 
    - 進到 **Preferences** 頁面，設定字型和語言
        - Language and Appearance/Fonts and Colors：將字體更改為 **Noto Sans CJK TC**，使得中文標點符號能正常顯示
        - Language and Appearance/Language：將 **preferred language for displaying pages** 更改為 **Chinese (Taiwan) [zh-tw]**，讓維基百科能優先使用繁體中文顯示中文頁面

- ALSA
    - 安裝 `alsa-utils`
- Output of `pacman -Qe`

```
alsa-utils 1.2.2-1
base 2-2
efibootmgr 16-2
exo 0.12.11-1
fcitx 4.2.9.7-2
fcitx-chewing 0.2.3-2
fcitx-configtool 0.4.10-3
fcitx-qt5 1.2.4-3
firefox 74.0-2
garcon 0.6.4-1
grub 2:2.04-5
lightdm 1:1.30.0-2
lightdm-webkit2-greeter 2.2.5-2
linux 5.5.10.arch1-1
linux-firmware 20200224.efcfa03-1
man-db 2.9.1-1
networkmanager 1.22.10-1
noto-fonts 20190926-4
noto-fonts-cjk 20190409-1
noto-fonts-emoji 20191016-4
os-prober 1.77-1
sudo 1.8.31.p1-1
terminus-font 4.48-1
thunar 1.8.12-1
thunar-volman 0.9.5-2
tumbler 0.2.8-1
vim 8.2.0343-1
xdg-user-dirs 0.17-2
xf86-video-vesa 2.4.0-2
xfce4-appfinder 4.14.0-1
xfce4-panel 4.14.3-1
xfce4-power-manager 1.6.6-1
xfce4-session 4.14.1-1
xfce4-settings 4.14.2-1
xfce4-terminal 0.8.9.1-1
xfconf 4.14.1-1
xfdesktop 4.14.2-1
xfwm4 4.14.0-1
xfwm4-themes 4.10.0-3
xorg-docs 1.7.1-2
xorg-fonts-100dpi 1.0.3-4
xorg-fonts-75dpi 1.0.3-4
xorg-luit 1.1.1-3
xorg-server 1.20.7-1
xorg-server-common 1.20.7-1
xorg-server-devel 1.20.7-1
xorg-server-xephyr 1.20.7-1
xorg-server-xnest 1.20.7-1
xorg-server-xvfb 1.20.7-1
xorg-server-xwayland 1.20.7-1
xorg-sessreg 1.1.2-1
xorg-setxkbmap 1.3.2-1
xorg-smproxy 1.0.6-2
xorg-x11perf 1.6.1-1
xorg-xbacklight 1.2.3-1
xorg-xcmsdb 1.0.5-2
xorg-xcursorgen 1.0.7-1
xorg-xdpyinfo 1.3.2-3
xorg-xdriinfo 1.0.6-1
xorg-xev 1.2.3-1
xorg-xgamma 1.0.6-2
xorg-xhost 1.0.8-1
xorg-xinput 1.6.3-1
xorg-xkbcomp 1.4.3-1
xorg-xkbevd 1.1.4-2
xorg-xkbutils 1.0.4-3
xorg-xkill 1.0.5-1
xorg-xlsatoms 1.1.3-1
xorg-xlsclients 1.1.4-1
xorg-xpr 1.0.5-1
xorg-xprop 1.2.4-1
xorg-xrandr 1.5.1-1
```
### Screenshot
![](https://i.imgur.com/PvmmCrh.png)
