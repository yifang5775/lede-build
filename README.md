# LEDE ESXi 旁路由在线编译

源码：https://github.com/coolsnowwolf/lede

目标：x86_64 Generic，BIOS 启动，SquashFS，1024 MiB 根分区，VMDK 和 IMG 镜像。

插件：SSR Plus+（Xray、SSR 客户端）、ZeroTier、NPS 管理界面及 NPC 客户端。没有内置订阅、服务器地址或密钥。

## 在线编译

进入 Actions → Build LEDE for ESXi → Run workflow。修改工作流程或编译配置也会触发编译。
成功后下载 lede-esxi-x86_64-bios。build-diagnostics 是日志，不是固件。
固件保存 14 天，诊断日志保存 7 天。源码与 feeds 的具体提交记录在成品的 build-info 中。

首次构建尚未成功前，不代表固件可用。完整编译可能受云端时间、磁盘空间或上游代码问题影响，需要根据日志处理。

## 安装前

保留旧 OpenWrt 虚拟机并备份配置，先用新虚拟机测试，不建议跨多个大版本直接恢复整份旧配置。
固件保留源码默认网络设置，没有预设旁路由 IP。首次启动应连接隔离测试端口组，避免默认 DHCP 影响现有网络。
确认新地址无冲突、关闭 LAN DHCP 后再接入实际 LAN。主路由网关为 192.168.88.1。
虚拟机应设置为 BIOS 启动。源码生成的 VMDK 可能需要用 ESXi 的 vmkfstools -i 转换后挂载。
安装时另行确认硬盘控制器、网卡和接口映射。

## NPC

LuCI 中的 NPS 插件使用 NPC 客户端连接远程服务端。具体版本取决于 packages feed，需与远程服务器协议兼容。
服务器地址、密钥、ZeroTier 网络和 SSR Plus+ 订阅在安装后填写。
