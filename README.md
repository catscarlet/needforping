# # Need For Ping
## 简介
这是我上传到Github的第一个项目，也是我第一个自己亲自手写的Web项目。

项目工作方式是定时ping远端主机，收集丢包率和延迟，并用图表展示。

项目是靠html+js+bash(unix shell)编写和运行的，

底层依靠shell脚本做数据的采集，界面使用一个简单的html，和highcharts JS图表库进行展示。

**项目更新版：Need For Ping 2[https://github.com/catscarlet/needforping2](https://github.com/catscarlet/needforping2)**

### 软件截图
![needforping logo](https://github.com/catscarlet/needforping/blob/master/snapshot.png)

## 安装
- 部分shell脚本需要修改其安装目录：
- needforping_DIR=/var/www/needforping
- 给$needforping_DIR/shell/目录下的shell脚本增加运行权限
- 手动在crontab中添加类似：

  ```
  */2 *   * * *   root    /var/www/needforping/shell/needforping.sh
  */2 *   * * *   root    /var/www/needforping/shell/output100json.sh
  ```

建议先自己看一下脚本内的实现，并不是很复杂

## 配置
修改output100json.sh中的OUTPUTNUM=720以调整每次更新json中的条目数量。目前是2分钟执行一次，设置为30便是一小时，设置720便是一天。ooutput100json是每2分钟执行一次，非实时生效。get___.js脚本未限制输出数量。

## 代码组成
### Bash部分
needforping.sh每2分钟向目的机ping 100个包，并收集延迟和丢包率。

output100json.sh每2分钟将收集到的数据转换为json文件。

### js部分
getLOSS.js和getLATENCY.js是我自己写的，用于获取和展示图表。JS图表库使用的是highcharts [http://www.highcharts.com/。](http://www.highcharts.com/。)

## 项目展示
展示地址：[http://pi.catscarlet.com:8091/needforping/](http://pi.catscarlet.com:8091/needforping/)

这个地址是一个插在北京联通宽带上的树莓派，偶尔会关机，所以打不开很正常。

## 已知缺陷
- 目前所有记录都是存在于$needforping_DIR/shell/pingresult目录下的，很多很臃肿，而且没有回滚功能，终究将会越跑越慢，甚至撑死硬盘。想要改良的话就需要linux的logrotate或crontab做支持，懒。有需要的朋友就自己修复吧。
- 脚本output100json.sh在首次运行的时候会有找不到文件的问题，没有做文件存在检查。
- js在请求JSON的时候，因为数组内容是从函数名中读取的，函数每次取数组都读一次文件，导致同一文件被读取很多次，增加服务器开销，降低读取速率。

## TODO
未来版本将会制作成html+php+mysql+js+bash多语言的，将采集的数据保存在mysql中，并使用php进行调用和回滚。新版本会修复上述已知缺陷。本项目主要是用于学习目的，实际使用请自行修改。
**项目更新版：Need For Ping 2[https://github.com/catscarlet/needforping2](https://github.com/catscarlet/needforping2)**
