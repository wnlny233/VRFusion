1.查看进程的完整启动命令：
ps -fp PID
cat /proc/PID/cmdline

2.定位执行文件和工作目录：
ll /proc/PID/exe
ll /proc/PID/cwd

3.查看打开的文件和网络连接：
lsof -p PID

4.实时监控io情况：
iotop -p PID
pidstat -d 1

5.跟踪系统调用：
strace -p PID -c

这是在办公室完成的文档；