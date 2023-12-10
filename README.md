# how to use

## build
```shell
clang -O2 -target -bpf -I./include -g -c src/mptcp_bpf_minrtt.c -o mptcp_bpf_minrtt.o
```

## bpftoolをコンパイルする
- Kernel リポジトリでbpftoolをmake, make install
- `sudo ln -s /usr/include/x86_64-linux-gnu/asm /usr/include/asm` する
- `sudo apt-get install -y gcc-multilib libbpf-dev`


## loadする
```shell
sudo bpftool struct_ops register mptcp_bpf_minrtt.o
```

結果の確認
```shell
sudo bpftool prog list
```

## sysctlで有効化する
```shell
sudo sysctl -w net.mptcp.scheduler=bpf_minrtt
```
## unload
```shell
sudo bpftool struct_ops unregister name bpf_minrtt
```
## tips
### printk
ebpfのプログラムのデバッグにどうぞ
```show
mount -t debugfs none /sys/kernel/debug
cat /sys/kernel/debug/tracing/trace_pipe
```

### ssでTCPコネクションをみる
```shell
ss show
ss -ntieOH
```
