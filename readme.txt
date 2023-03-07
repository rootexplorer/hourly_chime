
----

工作的原因，经常在电脑面前一坐几个小时，自己深知这是对自己的健康十分不负责任的一种行为方式，所以我想到让电脑每小时提醒我一下，于是就有了这个小项目。
每隔一段时间提醒一下，其实可以是隔任意长的时间，只是我为了同时也可以当作时钟来用，所以就搞成了整点报时。
整点报时的功能在很多挂钟上都有，但是放在电脑上时就有些没那么流行了。我知道的在windows上有不少恶意软件就在钻这个空子，里面植入的木马、病毒、广告数不胜数，好在我不用windows...

----

本项目基于 linux 系统，并不对硬件平台作任何限制，只要是能运行 linux 的硬件平台都行。而且对于发行版也没有特殊的要求，不管是redhat系的，还是debian系的，还是别的都可以。这意味着软路由、树莓派之类的都可以使用。

----

软件依赖：
cron 可以是 cronie 或者 别的，只要是 cron 就行 ， 本程序默认使用 cronie ，使用其它时就对应的修改 cron 文件
shell 可以是 sh 也可以是 bash 也可以是别的 ， 本程序默认使用 sh ，使用其它时就对应的修改 cron 文件
mplayer 这是一个播放器，如果你有自己青睐的播放器也是可以的，但请注意的是，我们这是在后端运行，所在前端很漂亮的播放器并不能发挥它的特长，而且不同的软件作者的逻辑设定不会相同，也许会出现前端界面打不开就直接退出的情况，所以选择时要选择可以在命令行中运行的播放器。本程序默认使用 mplayer ，使用其它时就对应的修改 cron 文件
XDG 这个... 提供执行权限，没有它的话，shell就不能成功调用播放器，并且提示没有权限。

----

使用方式：
1. 到需要定时播报的设备上安装所依赖的程序。因为不同的发行版有着不同的安装方式，所以我无法在此列出所有的安装方式。请自行查阅自己设备上的软件安装指南。

2. 下载 audios 里面的声音文件。当然，如果你觉得那个声音不好听，也可以自己录制。对于声音的格式是没有要求的，只要你选择的播放器能播放就行。
   放到设备的存储当中。比如我放在了 /mnt/apps/hourly_chime/audios/export/ 中
   
3. 建立日志文件夹。 我这里使用的日志文件夹是 /mnt/logs/hourly_chime/ 如果你有自己习惯的日志记录路径，请对应更改命令中的参数
sudo mkdir -p /mnt/logs/hourly_chime
sudo chmod 777 /mnt/logs/hourly_chime

4. 建立 cron 的定时任务
4.1. 查询用于播报的用户的 uid
     在 .sh 中执行时需要用到 XDG ，因此需要指定 XDG_RUNTIME_DIR，格式为 XDG_RUNTIME_DIR=/run/user/<uid> <command> ，其中 uid 的查询方式是 cat /etc/passwd | grep <username> 。
     例如：本例中用到的播报用户名是 expl
          所以查询命令是
cat /etc/passwd | grep expl
	  返回信息是
expl:x:1000:984::/home/expl:/bin/bash
	  所以我们得到 expl 的 uid 是 1000
4.2. 新建文件 /etc/cron.d/hourly_chime_cron 在里面写入以下内容，每行顶格开始写，可以直接复制，但要记得对应修改 用户名， uid ， 声音文件地址 ， 日志文件地址 这四种项目。
# hourly_chime's cron job
SHELL=/bin/sh
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=expl
HOME=/home/expl/
#
#
#
#
# 
#
0 0 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 15 /mnt/apps/hourly_chime/audios/export/00.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 1 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 15 /mnt/apps/hourly_chime/audios/export/01.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 2 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 15 /mnt/apps/hourly_chime/audios/export/02.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 3 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 15 /mnt/apps/hourly_chime/audios/export/03.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 4 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 15 /mnt/apps/hourly_chime/audios/export/04.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 5 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 15 /mnt/apps/hourly_chime/audios/export/05.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 6 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 15 /mnt/apps/hourly_chime/audios/export/06.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 7 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 35 /mnt/apps/hourly_chime/audios/export/07.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 8 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 50 /mnt/apps/hourly_chime/audios/export/08.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 9 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 50 /mnt/apps/hourly_chime/audios/export/09.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 10 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 50 /mnt/apps/hourly_chime/audios/export/10.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 11 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 50 /mnt/apps/hourly_chime/audios/export/11.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 12 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 50 /mnt/apps/hourly_chime/audios/export/12.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 13 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 50 /mnt/apps/hourly_chime/audios/export/13.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 14 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 50 /mnt/apps/hourly_chime/audios/export/14.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 15 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 50 /mnt/apps/hourly_chime/audios/export/15.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 16 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 50 /mnt/apps/hourly_chime/audios/export/16.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 17 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 50 /mnt/apps/hourly_chime/audios/export/17.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 18 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 50 /mnt/apps/hourly_chime/audios/export/18.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 19 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 50 /mnt/apps/hourly_chime/audios/export/19.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 20 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 50 /mnt/apps/hourly_chime/audios/export/20.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 21 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 50 /mnt/apps/hourly_chime/audios/export/21.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 22 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 25 /mnt/apps/hourly_chime/audios/export/22.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 23 * * * expl XDG_RUNTIME_DIR=/run/user/1000 mplayer -volume 20 /mnt/apps/hourly_chime/audios/export/23.wav >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_play.txt
0 0 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 1 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 2 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 3 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 4 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 5 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 6 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 7 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 8 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 9 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 10 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 11 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 12 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 13 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 14 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 15 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 16 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 17 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 18 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 19 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 20 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 21 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 22 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt
0 23 * * * expl date +'\%Y-\%m-\%d.\%H:\%M:\%S.\%N' >> /mnt/logs/hourly_chime/hourly_chime_`date +'\%Y-\%m-\%d'`_log.txt

4.3. 更改这个 cron 文件的权限
sudo chown root:root /etc/cron.d/hourly_chime_cron
sudo chmod 644 /etc/cron.d/hourly_chime_cron
     改完后我习惯性的查看一下
ls -laZh /etc/cron.d/hourly_chime_cron
cat /etc/cron.d/hourly_chime_cron

4.4. 如果你的 cron 需要重启才能生效的话，记得就要重启cron服务喔。

5. 给你的设备接上音箱吧。当然你也可以使用耳机，但耳机并不总是戴在耳朵上，会造成即使播报了你也没听见的情况。

