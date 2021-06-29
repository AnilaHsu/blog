---
title: "Linux 語法"
date: 2021-06-29
draft: true
categories: 
- Linux
tags:
- Linux
---
### 連進server

`ssh -p 5599 USER@HOST` 

-p 是指定 port ，若server 的不是預設的 port  ，則需要-p 指定的自己設定的port (有些為了避免機器人攻擊 port，因此會改成自己的 port )

## tumx

 shell 的管理工具 ****

```bash
# 新建 session
tmux
# 新建名為 xxx 的 session
tmux new -s xxx
```

```bash
# 連回 xxx session
tmux attach -t xxx
```

```bash
tmux ls
```

**在 tmux 的  shell  可使用組合鍵**

- 起手式 `ctrl + B`
- `"` : 水平分割
- `%` : 垂直分割
- `d` : 退出
- `[` : 歷史模式

### 查看 port

- 查詢所有 port 的使用狀況

    `ss -lp`

- 將前面的搜尋解果丟給後面的指令處理，grep 查詢符合的條件 ex :7321

    `ss -lp | grep :7321`

### 其他

一個程式可以使用多個 port ，但一個 port 只能一個程式使用

- 關掉該 port

    `kill -9 26247`

    -9代表強制 ; 26247為 pid