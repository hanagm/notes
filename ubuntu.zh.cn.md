# Ubuntu 使用笔记
- 压缩解压`tar`文件：
   ```text
   *.tar：tar -xvf PATH
   *.tar.xz：tar -xvJf PATH
   *.tar.gz：tar -xvzf PATH
   *.tar.bz2：tar -xvjf PATH
   ```
   `-c`参数意为创建压缩包，`-x`参数意为解压压缩包，`-v`参数意为显示输出信息，`-f`参数意为指定压缩包文件（待创建或待解压文件），`-t`参数意为列出压缩包中的文件（显示压缩包内容）
- 压缩解压`7z`文件：
   ```text
   a：添加文件到压缩包。例如：7z a test.7z ./data/*
   e：解压。例如：7z e ./test.7z
   l：显示压缩包内容。例如：7z l ./test.7z
   -m：设置压缩选项。“-mx -myx -m0=LZMA2:d=1g:fb=273”为极限压缩，“-ms=on”为启用固实压缩（默认），“-mmt=on”为启用多线程压缩（默认）。例如：7z a test.7z ./data/* -mx -myx -ms=on -mmt=on -m0=LZMA2:d=1g:fb=273
   ```
- 创建快捷方式：`ln ORIGINAL NEW`。`-s`参数意为创建软链接，`-f`参数意为删除已经存在的软链接。例如：`sudo ln -s /usr/bin/clang /usr/bin/x86_64-w64-mingw32-clang`
- 搜索文件：`find PATH -name "NAME"`。例如：`find /usr/bin -name "*clang"`
- 删除文件和文件夹：`rm PATH`。`-f`参数意为强制删除，`-r`参数意为递归删除（即删除文件夹）。例如：`rm -rf ./test/build`
- 创建文件夹：`mkdir NAME`。例如：`mkdir build`
- 下载文件：
   - `wget URL`：`-O`参数意为指定保存文件名，`-c`参数意为继续下载。例如：`wget -O test.zip https://www.ex.com/abc?=down`
   - `curl URL`：`-o`参数意为指定保存文件名，`--progress`参数意为显示进度条。例如：`curl -o test.zip --progress https://file.abc.com/down.asp`
- 列出文件：`ls PATH`。`-l`参数意为显示详细信息，`-h`参数意为显示人类易读的大小，`-s`参数意为在每个文件名后输出该文件的大小，`-A`参数意为显示除了“.”和“..”以外的所有文件，`-L`参数意为列出链接文件名而不是链接到的文件，`-N`参数意为不限制文件长度，`-Q`参数意为把输出的文件名用双引号括起来，`-R`参数意为列出所有子目录下的文件，`-d`参数意为将目录像文件一样显示，而不是显示其下的文件。例如：`ls -l -h /usr/bin`
- 设置环境变量：`export A=b:C=d`。不同变量之间以英文半角冒号`:`分隔，以英文半角美元符号`$`引用其他变量。例如：`export PATH=./eg1/bin:./eg2/bin:$PATH`
- 挂载和卸载：`mount SOURCE MOUNT_POINT`和`umount MOUNT_POINT`。`-r`参数意为只读挂载，`-w`参数意为读写挂载（默认）。例如：`mount -o loop ./test/soft.iso /mnt/iso1` -> `umount /mnt/iso1`
- 安装`.DEB`文件：`dpkg -i FILE`。例如：`sudo dpkg -i example.deb`
- 创建空文件：`touch PATH`。例如：`touch ./code/untitled.txt`
- 输出文件内容：`cat PATH`。例如：`cat ./test/cfg.ini`
- 复制文件和文件夹：`cp SOURCE TARGET`。`-f`参数意为强制覆盖，`-p`参数意为将文件属性也一同复制，`-r`参数意为递归复制（即复制文件夹），`-s`参数意为复制为软链接。例如：`cp -rf ./code ./test/code`
- 移动文件和文件夹：`mv SOURCE TARGET`。`-f`参数意为强制覆盖。例如：`mv -f ./code ./download/code`
- 重命名文件和文件夹：同`mv`命令。
- 清空控制台内容：`clear`。
- 包管理：
   ```text
   apt install：安装软件包
   apt search：搜索软件包
   apt purge：删除软件包，同时删除其所有配置文件
   apt autoremove：删除为了满足依赖而安装的，但现在已经不需要的软件包（但保留其配置文件）
   apt remove：删除软件包，但保留其配置文件
   apt autoclean：删除“/var/cache/apt/archives”文件夹下过期的安装包（清除过久的缓存）
   apt clean：删除“/var/cache/apt/archives”文件夹下所有的安装包（清除全部缓存）
   apt --fix-broken install：安装软件时出现任何依赖错误，都尝试使用此命令解决
   ```
- 修改默认程序：修改`/etc/gnome/defaults.list`文件。
- `PPA`的添加和删除：
   ```text
   apt-add-repository ppa:NAME：添加名为“NAME”的PPA
   apt-add-repository --remove ppa:NAME：删除名为“NAME”的PPA。此命令不会删除使用此PPA安装的软件包。
   #如果要删除PPA以及用它安装的软件包，要使用“ppa-purge”这个工具，要单独安装
   #ppa-purge ppa:NAME：删除名为“NAME”的PPA，同时删除此PPA安装的所有软件包
   ```
- 更新软件和系统：
   ```bash
   sudo apt update
   sudo apt full-upgrade
   ```
- 安装、更换和删除内核：
- 双显卡笔记本防止开机卡住安装方法+安装英伟达独显驱动：
   1. 安装系统之前先关闭`Security Boot`（安全启动，去`BIOS`里找）
   2. 屏蔽`Nouveau`开源显卡驱动：在`grub`界面，选中`Install Ubuntu`，按`e`进入命令行模式，在`quiet splash --`后面（也可能没有`-`），添加`acpi_osi=linux nomodeset`，然后按`F10`重新引导
   3. 重新引导之后，安装程序的窗口可能有一部分在屏幕下方，导致部分按钮无法点击。按下`Alt+F7`，鼠标会变成手指形状，此时将窗口向上拖动即可
   4. 安装完成，重启。在电脑重启黑屏的时候，拔出U盘（重启的时候也可能卡在 logo，所以在要求选择引导选项的时候，重复上述操作）
   5. 永久屏蔽`Nouveau`开源显卡驱动：
       ```bash
       sudo nano /etc/modprobe.d/blacklist.conf
       #在blacklist.conf文件最后添加下面两行后保存
       blacklist nouveau
       options nouveau modeset=0
       sudo update-initramfs -u #刷新内核
       #sudo gedit /boot/grub/grub.cfg
       #进行第二步的操作，然后保存
       #sudo update-grub #更新grub
       ```
   6. 安装英伟达独显驱动：
      ```bash
      #方法一（推荐）：
      ubuntu-drivers devices #查看显卡型号及建议安装的驱动
      sudo ubuntu-drivers autoinstall #自动安装推荐的驱动
      #方法二：
      sudo add-apt-repository ppa:graphics-drivers/ppa
      sudo apt update
      ubuntu-drivers devices
      sudo apt install nvidia-390 #从上一行命令的输出中选择合适的版本安装
      #方法三：
      #“软件和更新” -> “附加驱动” -> 安装驱动
      #安装成功后记得重启系统以生效
      ```
    7. 重启
- 安装常用软件：
   - 搜狗拼音输入法：
      1. 先安装`fcitx`，`fcitx-libs-qt`，`libfcitx-qt0`，`libopencc2`和`libqtwebkit4`
      2. 到官方网站 https://pinyin.sogou.com/linux/ 下载安装包后安装
      3. Region & Language -> Manage Installed Languages -> Keyboard input method system -> fcitx -> Apply System-Wide
      4. 去右上角系统托盘里找到键盘图标，右击打开菜单，Configure Current Input Method -> Input Method -> + -> Sogou Pinyin
   - 网易云音乐：到官方网站 https://music.163.com/#/download 下载安装包后安装
   - WPS：到官方网站 http://www.wps.cn/product/wpslinux 下载安装包后安装。字体到这里下载安装：http://mirrors.ustc.edu.cn/deepin/pool/non-free/t/ttf-wps-fonts/
   - TIM：http://mirrors.ustc.edu.cn/deepin/pool/non-free/d/deepin.com.qq.office/
   - 微信：http://mirrors.ustc.edu.cn/deepin/pool/non-free/d/deepin.com.wechat/ 和 http://mirrors.ustc.edu.cn/deepin/pool/non-free/d/deepin.com.weixin.work/
   - Telegram：包名为`telegram-desktop`，如果官方版本较老，新版本可在 https://desktop.telegram.org/ 下载安装
   - 迅雷极速版：http://mirrors.ustc.edu.cn/deepin/pool/non-free/d/deepin.com.thunderspeed/
   - 百度网盘：http://mirrors.ustc.edu.cn/deepin/pool/non-free/d/deepin.com.baidu.pan/
   - qBittorrent：包名为`qbittorrent`，如果官方版本较老，新版本可在 https://www.qbittorrent.org/download.php 下载安装
   - Steam：包名为`steam`
   - VLC Media Player：包名为`vlc`
   - screenfetch：包名为`screenfetch`
   - Free Download Manager：目前暂无 Linux 版，但我很期待这个软件出 Linux 版
   - 7-Zip：包名为`p7zip-rar`，`p7zip-full`和`p7zip`（都要安装）
   - 有道词典：http://mirrors.ustc.edu.cn/deepin/pool/non-free/y/youdao-dict/
   - 美化工具和通知栏：包名为`gnome-tweak-tool`和`libindicator`
   - Wine：安装深度移植的软件必须要安装这个：http://mirrors.ustc.edu.cn/deepin/pool/non-free/d/deepin-wine/ http://mirrors.ustc.edu.cn/deepin/pool/non-free/d/deepin-wine-helper/ http://mirrors.ustc.edu.cn/deepin/pool/non-free/d/deepin-wine-helper64/ http://mirrors.ustc.edu.cn/deepin/pool/non-free/d/deepin-wine-plugin/ http://mirrors.ustc.edu.cn/deepin/pool/non-free/d/deepin-wine-plugin-virtual/ http://mirrors.ustc.edu.cn/deepin/pool/non-free/d/deepin-wine-uninstaller/
- 安装专业软件：
   - 基本：包名为`build-essential`，`gdb`，`git`，`cmake`，`pkg-config`，`autoconf`，`automake`，`python`，`python3`，`perl`，`ruby`，`flex`，`bison`，`yasm`，`nasm`，`binutils`
   - VSCode：到官方网站 https://code.visualstudio.com/Download 下载合适的安装包安装
   - Qt Creator：包名为`qtcreator`，如果官方版本较老，新版本可在 http://mirrors.ustc.edu.cn/qtproject/official_releases/qtcreator/ 下载安装
   - VirtualBox：包名为`virtualbox`，如果官方版本较老，新版本可按照官方网站 https://www.virtualbox.org/wiki/Linux_Downloads 的指导来安装
   - Clang：包名为`clang`，`clang-tools`，`lldb`，`lld`，`libfuzzer-N-dev`，`libc++-dev`，`libc++abi-dev`，`libomp-dev`（`N`为具体的主版本号，例如：7、8和9等），如果官方版本较老，新版本可按照官方网站 http://apt.llvm.org/ 的指导来安装
   - MinGW-w64：包名为`mingw-w64`，`mingw-w64-common`，`mingw-w64-i686-dev`，`mingw-w64-tools`，`mingw-w64-x86-64-dev`。具体请查看官方网站 https://launchpad.net/ubuntu/+source/mingw-w64
   - glibc：包名为`libc6-dev`，`libc6-dev-amd64`，`libc6-dev-amd64-cross`，`libc6-dev-amd64-i386-cross`，`libc6-dev-i386`，`libc6-dev-i386-amd64-cross`，`libc6-dev-i386-cross`（不知道具体哪个有用，我就都写下来了）
   - Linux headers：
   - GCC/G++：包`build-essential`自带
   - ICC：按照官方网站 https://software.intel.com/en-us/qualify-for-free-software/opensourcecontributor 的指导下载安装
- 安装使用代理：
   1. 安装`shadowsocks-qt5`：
      ```bash
      sudo add-apt-repository ppa:hzwhuang/ss-qt5
      sudo apt update
      sudo apt install shadowsocks-qt5
      ```
      经过我的测试，用这个方法成功的次数太少，可以直接去作者的 GitHub 下载 snap 版本：https://github.com/shadowsocks/shadowsocks-qt5/releases
    2. 设置节点：
       ```text
       Local Address：127.0.0.1
       Local Port：1080
       Local Server Type：HTTP(S)
       Automation：Auto connect on application start
       #其他选项按你的节点来
       ```
    3. 设置系统代理，**IP地址**均为`127.0.0.1`，**端口**均为`1080`
    4. 设置浏览器代理，IP地址与端口的设置与上一步的相同（所有协议都要这样设置）
    5. 软件里点击Connect
- 常用分区方案：

   | 挂载点 | 文件系统 | 推荐大小 | 分区介绍 |
   | ----- | -------- | ------- | ------- |
   | / | ext4 | 15~20GB | 根目录，相当于 Windows 的 C 盘，用于存放系统文件。此外，所有软件也都会安装到这个分区，如果你可能安装很多软件，要适当调整这个分区的大小 |
   | /home | ext4 | 个人桌面用户建议此分区尽可能的大 | 主目录，用户的所有个人文件均存放在这个分区 |
   | /boot | ext4 | 用不着太大，但也不能太小，建议1GB | 引导分区，系统存放内核的地方 |
   | /swap | swap | 当你的物理内存<8GB时建议为物理内存的两倍，否则为物理内存的大小 | 交换空间，相当于 Windows 的虚拟内存 |

   注：如果你只有一块硬盘，并且也不用考虑与其他操作系统（比如 Windows）共存的问题，可以只创建一个根分区。
- 常用快捷键（终端）：

   | 按键 | 作用 |
   | ---- | ---- |
   | Ctrl + Alt + T | 打开终端 |
   | Tab | 命令或文件名自动补全 |
   | F11 | 切换全屏/窗口 |
   | Ctrl + Shift + C | 复制 |
   | Ctrl + Shift + V | 粘贴 |