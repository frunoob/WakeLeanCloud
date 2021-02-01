## 这是干嘛的？

这个项目主要是用来解决LeanCloud通过定时任务唤醒机器时被流控的问题。

这个项目基于blogimg/WakeLeanCloud修改而成，和原项目的主要区别在于其是通过发表`wakeup`评论来唤醒，解决了需要绑定域名的问题。

## 如何使用

1. Fork此项目
2. 添加一个名为`GITHUB_TOKEN`的Token，并为赋予`repo`，`admin:repo_hook` ， `workflow`的权限
3. 添加名为`APPID` `APPSECRETS`的Secrets，内容为自己的LeanCloud的AppID和*MasterKey*，需要MasterKey的原因是要删除评论，保存在Github的Secrets中不会泄露，因此可以放心使用

详细使用方法见：https://hiram.wang/leancloud-timer-github-actions/

## 修改时间

修改时间直接修改`/.github/workflows/autoWakeup.yml`文件的cron表达式即可。

*时间是UTC时间，注意时差问题*

## 优雅执行

默认fork后是每天早上7点执行。并且每次执行都会push新的记录。当然你可以删掉。

这里提供两种运行思路，请各位博主自行决定如何运行。

1. 不改动（默认情况下）

   这种方式一次设置好就OK了，理论上不会出现问题。但是由于每次都会commit新记录，因此可能会很烦。

2. 运行时间为`"0 0 * * *"`

   这种代表每天8:00执行一次，然后通过LeanCloud的定时函数。这种方法思想很简单：既然自唤醒函数不能唤醒休眠的机器，那么就用脚本从外部唤醒，然后用定时任务在内部唤醒。

