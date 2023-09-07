# bpftop

### 简介

bpftop是个依赖于bpftool的展示BPF程序的占用的简单Python程序.

可以展示:

* 加载BPF程序的进程PID:FD
* BPF 程序ID
* BPF 程序名称
* 每秒占用的时长, 可以简单等效为CPU占用. 比如CPU有16个, 这个字段为0.5, CPU占用即为 0.5/16
* BPF 程序每个sample_interval的运行次数

主要用于排查最大占用的bpf程序是哪个程序加载的.

截图:

![简单展示](https://github.com/XinShuichen/bpftop/assets/26585883/d8269b0d-a824-4a42-ae11-b40ec3a5e53d)

** 注意: ** 这个仅仅计算了bpf指令执行的时间, 不包括hook本身的开销, 比如kprobe和kretprobe本身int3的开销. 这是内核功能决定的.

### 依赖

依赖于 bpftool, python3 以及Linux有/proc/sys/kernel/bpf_stats_enabled文件(即有bpfstat统计功能).

```shell
echo 1 > /proc/sys/kernel/bpf_stats_enabled
```

### 用法

TOP: 默认是-1, 展示所有的bpf程序.
SAMPLE_INTERVAL: 采样间隔, 在bpf程序特别多的时候用于减小cpu占用. 默认是1s.

```
usage: bpftop [-h] [--sample-interval SAMPLE_INTERVAL] [--top TOP]

Monitor BPF program run time and run count

optional arguments:
  -h, --help            show this help message and exit
  --sample-interval SAMPLE_INTERVAL
                        Sample interval in seconds
  --top TOP             Number of top programs to display
```

平时直接`bpftop`即可. 如果
```
$ cat /proc/sys/kernel/bpf_stats_enabled
0
```
则需要
```
echo 1 > /proc/sys/kernel/bpf_stats_enabled
```
来打开bpfstat统计.
