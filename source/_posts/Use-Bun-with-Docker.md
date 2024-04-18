---
title: Use Bun with Docker
date: 2024-04-18 10:30:36
tags: docker, bun.js
---
Hi all, 這次的發想單純是因為假日到了找個工具來玩玩，但又發現要安裝依賴套件又有點會污染到本機端的環境，於是才有了這篇文章，這篇文章的目標就是透過 docker 把對於 bun.js 的依賴與本機端坐個區隔。

首先稍微介紹什麼是docker，由於網路上對於這套工具的解說已經非常多了，在這邊就簡單地講述。

>Docker 是一個開源平台，用於自動部署、擴展和管理應用程序，通過容器化技術將應用程序與其環境打包在一起，以確保在任何地方的運行都具有一致性。

既然 docker 是個連同環境一起打包的一種工具，那我們只需要把 已經安裝好 bun這個指令的環境打包起來做使用就好啦，況且目前就已經有現成的image可做使用，以下為 docker 拉取 image指令

`$ docker pull oven/bun`

拉取完畢後就可以思考該怎麼做使用了，以下是我們在使用上的需求點
- 能夠同步主機端的專案
- 可以順利 build好並可在主機端進行連線
- 可以隨時脫離前景服務 ( ctrl+c 跳出)

依照上面的需求點可以這樣下指令

`$ docker run -it --rm -p 8080:8080 -w /app -v $(pwd):/app oven/bun bun <custom-command>`


到這邊我們已經可能正常的透過 docker 使用 bun了，但每次要使用 bun 都必須打這麼長一大串是蠻不友善的，於是我們可以在各自的 .bashrc/.zshrc 寫下這個 function

``` shell=
bun() {
  docker run -it - rm -w /app -v "$(pwd)":/app -p 8080:8080 oven/bun bun "$@"
}
```
這樣一來我們就可以使用指令初始化專案:  `$ bun init`

## Conclusion
透過上面流程，我們就可以透過 docker 把開發環境與我們的本機端環境做切割。但這個方法不侷限於使用 bun，同理的 python, nodejs, npm 等等都可透過這個方法去做環境區分，那麽這邊就先這樣囉。
