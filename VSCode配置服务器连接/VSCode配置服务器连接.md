
## Motivation
实验室有一台Ubuntu系统的服务器（也可以说是一台Ubuntu系统的电脑吧），主要用于深度学习的实验，配有一块1080ti的显卡。

之前的使用方法一直是将其作为一台台式机使用，配置了显示屏、鼠标键盘。

但是，个人平时还是用笔记本多一些，但是奈何显卡不给力，如果要跑实验又要切换到服务器上。

想一想一张桌子上需要多放一套键鼠和显示屏，得有多拥挤！还有就是其他同学需要使用服务器不太方便。

还有很不喜欢使用Xshell连接服务器使用，谁愿意对着一个黑乎乎的界面写代码？（熟练使用Vim者除外？）

在之前有文章提出使用Pycharm+Docker来进行操作，我试过几次，还是没有配置成功!同时并不是很喜欢使用Pycharm,而是偏爱VSCode。

在最近（应该已经过了很久了）VSCode推出了插件Remote SSH，对于远程的服务器连接和开发非常方便。我自己体验过感觉也很Nice（尤其是配置成秘钥连接后，少去了每次输入密码的麻烦）。在腾讯云和DigitalOcean的linux服务器上，都使用过！

因为服务器装的系统是Ubuntu desktop的版本，之前一直以为Remote SSH只能连接 Ubuntu Server的版本，所以一直没有尝试使用这个插件去连接局域网的Linux服务器。

今天忽然心血来潮，想试试使用VSCode的Remote SSH这个插件来试试连接局域网内的桌面版的Ubuntu系统电脑。
以下笔记本电脑代表local client、linux服务器表示remote server。

## Simulation 

1. 使用用户名和密码登录linux服务器，打开终端输入命令```ifconfig | grep inet```
可以得到linux服务器的一些ip地址，我们主要选择192.168开头的地址，在我这里为 192.168.1.110
2. 在笔记本电脑和linux服务器的终端中是尝试使用一下ssh命令，保证两边的ssh都是可以通的！
3. 在笔记本电脑的终端中使用命令```ssh wxzhu@192.168.1.110``` wxzhu为我的linux服务器登录的用户名，出提示输入密码。PS：该笔记本应该与Linux服务器在同一个局域网内，最简单的方法就是连接同一个路由器的网络
4. 后面的方法就与Remote SSH连接远程服务器类似了。
   1. 笔记本电脑的VSCode安装Remote SSH的插件
   2. 配置SSH:

    使用Xshell生成一对秘钥（包括公钥和秘钥），很多教程喜欢使用 ```ssh-keygen -t rsa```,但是我自己比较喜欢Xshell。具体方法大致为:Xshell菜单中的工具->用户秘钥管理者->生成。再通过```scp```命令将公钥发送到服务器端
    最后在笔记本的VSCode中配置Remote SSH的设置。
    ```config
    Host 本地ubuntu
        HostName 192.168.1.110
        User wxzhu
        IdentityFile C:\Users\WenxingZhu\.ssh\Local_Ubuntu
    ```
    IdentityFile后面是指秘钥文件所在位置 Host的名字可以随意起
    
   3. 连接服务器
5. 最后的效果就是你会觉得像在本地写代码一下，但是编辑的代码都是linux服务器中的代码！***这个也是和Pycharm的远程开发不同的地方，Pycharm还需要设置一个本地路径和远程路径的映射，本质上代码还是写在了本地，再通过文件传输协议上传到服务器，可能会出现同步的问题***



