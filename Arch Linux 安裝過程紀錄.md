# __Arch Linux 安裝過程紀錄__

## __進入開機媒體__

- 鍵盤配置
    - 保留預設值即可 (US)    

- 連線到網際網路
    - 使用 `ip a` 檢查網卡是否正常運作
    - 使用 `wifi-menu` 連上無線網路
    - 使用 `dhcpcd` 連上有線網路 (動態 ip)
    - 使用 `ping` 確認網路連線

- 編輯 `/etc/pacman.d/mirrorlist`，新增海洋大學映像站
    
    ```
    Server = http://shadow.ind.ntou.edu.tw/archlinux/$repo/os/$arch
    ```

- 安裝其他套件
    - `networkmanager`
    - `grub`
    - `man`

## __After installation (系統設定)__

- 網路連線
    - `nmtui`

- 使用者管理
    - `pacman -S sudo`
    - 新增 billson 使用者
    - 將 billson 加入 **wheel** 群組
    - 編輯 `/etc/sudoers`，執行 `EDITOR=vim visudo`

- 安裝桌面環境

    ```
    pacman -S xfce4 xorg-server lightdm lightdm-webkit2-greeter
    ```

    - 啟用 lightdm `systemctl enable lightdm`
    - 下次開機時，直接進入圖形化界面 `systemctl set-default graphical.target`
    - 編輯 `/etc/lightdm/lightdm.conf`，設定 `greeter`
        
        ``` yaml 
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
    ```
    pacman -S ttf-liberation noto-fonts noto-fonts-cjk noto-fonts-emoji terminus-font
    ```
    
    ```
    pacman -S fcitx-im fcitx-chewing fcitx-configtool
    ```
    
    - 編輯 `~/.xprofile`
        
        ``` bash
        export XMODIFIERS=@im=fcitx
        export GTK_IM_MODULE=fcitx
        export QT_IM_MODULE=fcitx
        fcitx&
        ```        

- 安裝網頁瀏覽器 **Firefox** 
    - 進到 **Preferences** 頁面，設定字型和語言
        - Language and Appearance/Fonts and Colors：
        
            將字體更改為 __Noto Sans CJK TC__，使得中文標點符號能正常顯示

        - Language and Appearance/Language：
            
            將 __preferred language for displaying pages__ 更改為 __Chinese (Taiwan) [zh-tw]__，讓維基百科能優先使用繁體中文顯示中文頁面

- ALSA
    - `pacman -S alsa-utils`

- PulseAudio
    - `pacman -S pulseaudio  pulseaudio-alsa pavucontrol`

## __桌面應用__

|類別|套件|
|-|-|
|File manager|thunar <br> thunar-archive-plugin <br> thunar-media-tags-plugin <br> thunar-volman <br> tumbler <br> gvfs|
|Archieve Manager|file-roller|
|Programming|VS code (`code`)|
|Video and Audio|vlc <br> lollypop *|
|Image|viewnior|
|Office|LibreOffice (`libreoffice-fresh`) <br> qpdfview|
|Disk|gparted <br> baobab <br>|
|XFCE panel plugins|xfce4-clipman-plugin <br> xfce4-mount-plugin <br> xfce4-notifyd <br> xfce4-pulseaudio-plugin <br> xfce4-whiskermenu-plugin|
|Others|xfce4-screenshooter <br> xfce4-taskmanager <br> xfce4-screensaver <br> zenity *|

<br>

> \* lollypop 需要安裝可選擇的相依性套件才能播放音樂
> ```
> Optional Deps:
> easytag: Modify tags
> gst-libav: FFmpeg plugin for GStreamer [installed]
> gst-plugins-bad: "Bad" plugin libraries [installed]
> gst-plugins-base: "Base" plugin libraries [installed]
> gst-plugins-good: "Good" plugin libraries [installed]
> gst-plugins-ugly: "Ugly" plugin libraries [installed]
> kid3-qt: Store covers in tags
> libsecret: Last.FM support [installed]
> python-pylast: Last.FM support
> youtube-dl: Youtube support
> ```

> \* JUCE Framework 需要這個套件 [參考連結](https://forum.juce.com/t/native-filechooser-not-used-on-linux-xfce/26347)

## __桌面主題__
- __GTK 應用程式主題__
    - 把 __GTK2/GTK3__ 主題放在 `~/.themes` 目錄下
    - 把 __Icon__ 主題放在 `~/.icons` 目錄下
    - 讓特定的程式使用不同的 GTK 主題
        
        可以透過修改 __Desktop entry__ 來達到這個目的，系統的 __desktop files__ 基本上都是放在 `/usr/share/applications/` 這個目錄下，使用者的則是放在 `~/.local/share/applications`。        
        
        以 LibreOffice Writer 為例，複製 `libreoffice-writer.desktop` 到自己的家目錄下，並修改 `Exec` 的值：
        
        語法：`Exec=env GTK_THEME={theme_name}{commmand}`

        ``` yaml
        [Desktop Entry]
        Version=1.0
        Terminal=false
        Icon=libreoffice-writer    
        ...
        Exec=env GTK_THEME=vimix-beryl libreoffice --writer %U
        ...
        ...
        Actions=NewDocument;
        [Desktop Action NewDocument]
        Name=New Document
        ...
        Exec=env GTK_THEME=vimix-beryl libreoffice --writer
        ```

- __Qt / KDE / Kuantum 應用程式主題__
    - Xfce桌面環境只能管理 GTK 主題，需要安裝額外的管理程式：

        ```
        pacman -S qt5ct kvantum-qt5
        ```

    - 設定環境變數
        
        `~/.xprofile`:

        ``` bash
        export QT_QPA_PLATFORMTHEME=qt5ct
        ```
    
    - 設定主題

        執行 `qt5ct`，在 __Appearance__ 的頁籤把 __Style__ 選項設為 __kvantum__，接著開啟 __Kvantum Manager__，選擇 __KvGnomeDark__ 作為預設主題，也可以進到 __Application Themes__ 頁籤，為特定的應用程式套用不同的主題

## JACK Audio Connection Kit
- 安裝：

    ```
    pacman -S jack2 a2jmidid pulseaudio-jack jack_capture realtime-privileges cadence
    ```

- 把使用者加進 __audio__ 和 __realtime__ 群組：

    ```
    usermod -G realtime,audio -a billson
    ```

## __Screenshot__

![](Screenshot/Screenshot_2020-03-27_23-43-54.png)

![](Screenshot/Screenshot_2020-03-28_10-32-31.png)

![](Screenshot/Screenshot_2020-03-22_10-44-05.png)

![](Screenshot/Screenshot_2020-03-27_23-41-20.png)

![](Screenshot/Screenshot_2020-03-27_23-45-52.png)

