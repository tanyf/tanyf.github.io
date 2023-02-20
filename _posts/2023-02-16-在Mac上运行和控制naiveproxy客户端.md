---
layout: post
title: 在Mac上运行和控制naiveproxy客户端
description: Launchd的基础知识和使用它控制naiveproxy客户端
tags: naiveproxy launchd plist deamon agent
---


# 在Mac上运行和控制naiveproxy客户端
## Launchd
类似于Linux上的Systemd, 用于控制启动和检测运行各种服务，使用`lauchctl`命令来控制。
### Daemons和Agents
Launchd管理下主要有两种类型：Daemons和Agents。这两种类型分别存放在不用的文件夹下。Agents一般是用户登录后执行，Daemons是开机后就执行，可以通过`UserName`指定用户（比如`root`)

| 类型 | 路径 | 说明 |
|----------|----------|----------|
| User Agents | ~/Library/LaunchAgents | 用户 Agents 当前用户登录时运行 |
| Global Agents | /Library/LaunchAgents | 全局 Agents 任何用户登录时都会运行 |
| System Agents | /System/Library/LaunchAgents | 系统 Agents 任何用户登录时都会运行 |
| Global Daemons | /Library/LaunchDaemons | 全局 Daemons 内核初始化加载完后就运行 |
| System Daemons | /System/Library/LaunchDaemons | 系统 Daemons 内核初始化加载完后就运行 |

### naiveproxy的 .plist文件
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>Label</key>
	<string>xyz.tanyif.naive</string>
	<key>RunAtLoad</key>
	<true/>
	<key>UserName</key>
	<string>yifengtan</string>
	<key>StandardErrorPath</key>
	<string>/Users/yifengtan/naiveproxy/stderr.log</string>
	<key>StandardOutPath</key>
	<string>/Users/yifengtan/naiveproxy/stdout.log</string>
	<key>WorkingDirectory</key>
	<string>/Applications/naiveproxy-v109.0.5414.74-1-mac-arm64</string>
	<key>ProgramArguments</key>
	<array>
		<string>/Applications/naiveproxy-v109.0.5414.74-1-mac-arm64/naive</string>
		<string>/Applications/naiveproxy-v109.0.5414.74-1-mac-arm64/config.json</string>
	</array>
	<key>KeepAlive</key>
	<true/>
</dict>
</plist>
```

### launchctl常用命令
`load`<.plist path>: 加载一个任务。

`start`: 一般的任务在`load`后要再运行，如果在.plist中设置了`RunAtLoad`或者`KeepAlive`则在`load`时候就启动了。

`list`: 列出当前加载的任务。

`stop`: 终止一个正在进行中的任务。

`unload`: 指定路径卸载一个任务（注意：在一个.plist出错后做了一些修改，一定要记得先`unload`,再重新`load`,这样才能将修改生效）



#naiveproxy #launchctl #plist