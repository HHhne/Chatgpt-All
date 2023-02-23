---
typora-root-url: /images/ChatGPT合集

---

# 						ChatGPT合集

> 1.ChatGPT Telegram Bot
>
> 2.反代网站
>
> 3.伪终端
>
> ​	由于Openai的不断更新接口拦截，现在实现chatgpt对接要麻烦了很多，在此分享下在Github上收集的优秀项目，和搭建过程-

## 1.Telegram机器人

[**ChatGPT Telegram Bot**](https://github.com/RainEggplant/chatgpt-telegram-bot)：一个基于 Node.js 的 Telegram ChatGPT 机器人，支持无浏览器和基于浏览器的 API。

#####  目前关于ChatGPT的API调用有以下几种方式:![image-20230223164925186](1.png)

> - 官方的。使用text-davinci-003，通过OpenAI官方的完成度API来模仿ChatGPT（最强大的方法，但它不是免费的，也没有使用为聊天而微调的模型）。
> - 非官方的。使用非官方代理服务器，以规避Cloudflare的方式访问ChatGPT的后端API（使用真正的ChatGPT，而且相当轻量级，但依赖于第三方服务器，而且有速率限制）
> - 浏览器（不推荐）。使用Puppeteer访问ChatGPT的官方网络应用（使用真正的ChatGPT，但非常不稳定，重量级，容易出错）。

### 搭建

<!--需要注意的是，本文使用的是Node18的生产环境。如果搭建失败，建议更新至Node18,。-->

下载并解压项目文件后，在config文件夹下创建local.json文件，可复制default.json作为模板，并按照说明修改local.json，其中的设置将覆盖default.json中的默认设置。

```wiki
下面三种模式:
基于浏览器的API，将api.type设置为browser 。然后提供OpenAI/谷歌/微软的API和其他设置。需要安装Chromium的浏览器。

基于无浏览器的官方API，将api.type设置为official。然后提供你的OpenAI API密钥和其他设置。收费

基于无浏览器的非官方API，将api.type设置为非官方。然后提供你的OpenAI访问令牌（如何获得你的访问令牌？）和其他设置。
```

```bash
#执行
pnpm install
pnpm build && pnpm start  #关于该机器人的更多细节参考原作者介绍
```

这里选择第三种非官方API，详情参考文档-->[chatgpt-api](https://github.com/transitive-bullshit/chatgpt-api#access-token)

这里介绍两种获取Access Token的方法:

1.使用 [acheong08/OpenAIAuth](https://github.com/acheong08/OpenAIAuth)，这是一个python脚本，可以自动登录并获得访问令牌。这适用于电子邮件+密码账户（例如，它不支持通过微软/谷歌授权的账户）。

2.你可以通过登录ChatGPT网络应用程序手动获取访问令牌，然后打开https://chat.openai.com/api/auth/session，这将返回一个包含访问令牌字符串的JSON对象。

这里也提供方法1所使用的[Python脚本的修改版](https://github.com/HHhne/ChatGPT-/blob/main/Token.py),请在代码最后填入邮箱和密码(不支持谷歌和微软邮箱认证式登录)

> Access tokens 持续时间约为8小时

搭建成功如下，[♡Chat-GPT](https://t.me/mmmin_bot)

![image-20230223174116372](/images/image-20230223174116372.png)

## 2.反代网站

[**Chatgpt-web**](https://github.com/Chanzhaoyu/chatgpt-web)：使用 `express` 和 `vue3` 搭建的支持 `ChatGPT` 双模型演示网页。

同样提供了两种模型

| 方式                                          | 免费？ | 可靠性 | 质量 |
| --------------------------------------------- | ------ | ------ | ---- |
| `ChatGPTAPI(GPT-3)`                           | 否     | 可靠   | 较笨 |
| `ChatGPTUnofficialProxyAPI(网页 accessToken)` | 是     | 不可靠 | 聪明 |

支持双模型，提供了两种非官方 `ChatGPT API` 方法

| 方式                                          | 免费？ | 可靠性 | 质量 |
| --------------------------------------------- | ------ | ------ | ---- |
| `ChatGPTAPI(GPT-3)`                           | 否     | 可靠   | 较笨 |
| `ChatGPTUnofficialProxyAPI(网页 accessToken)` | 是     | 不可靠 | 聪明 |

> ***Note:*** 网页 `accessToken` 存在大约 8 小时，而且国内地区网络问题更推荐使用 `GPT-3` 的方式
>
> 对比：
>
> 1. `ChatGPTAPI` 使用 `text-davinci-003` 通过官方`OpenAI`补全`API`模拟`ChatGPT`（最稳健的方法，但它不是免费的，并且没有使用针对聊天进行微调的模型）
> 2. `ChatGPTUnofficialProxyAPI` 使用非官方代理服务器访问 `ChatGPT` 的后端`API`，绕过`Cloudflare`（使用真实的的`ChatGPT`，非常轻量级，但依赖于第三方服务器，并且有速率限制）
>
> 切换方式：
>
> 1. 进入 `service/.env` 文件
> 2. 使用 `OpenAI API Key` 请填写 `OPENAI_API_KEY` 字段 [(获取 apiKey)](https://platform.openai.com/overview)
> 3. 使用 `Web API` 请填写 `OPENAI_ACCESS_TOKEN` 字段 [(获取 accessToken)](https://chat.openai.com/api/auth/session)
> 4. 同时存在时以 `OpenAI API Key` 优先

由于站点部署是前后端分离的，所以对于前端和后端需要分别进行安装依赖和开启服务

### 测试运行

##### 前端

根目录下执行以下命令

```bash
pnpm bootstrap
pnpm dev
```

##### 后端

`/service` 下运行以下命令

```bash
pnpm install
pnpm start
```

测试没有问题，开启tmux窗口

```bash
chmod +x ./start.sh && ./start.sh #最后退出tmux窗口即可 
```

最后附上Nginx反代配置代码

```nginx
server {
    listen       80;
    server_name 你的域名;
      location / {
        proxy_pass http://127.0.0.1:1002;#需要被方向代理的地址
        proxy_set_header Host $host:$server_port;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header REMOTE-HOST $remote_addr;
        add_header X-Cache $upstream_cache_status;
        proxy_set_header X-Host $host:$server_port;
        proxy_set_header X-Scheme $scheme;
        proxy_connect_timeout 30s;
        proxy_read_timeout 86400s;
        proxy_send_timeout 30s;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }}
```

部署成功如图,[Chatgpt-web](http://chat.hhhnee.top)

![image-20230223184552349](/images/image-20230223184552349.png)

## 3.伪终端

**[Pandora](https://github.com/pengzhile/pandora)**:一个命令行的`ChatGPT`,实现了网页版`ChatGPT`的主要操作。能过`Cloudflare`，理论上速度还可以。

### 运行

> - Python版本目测起码要`3.7`
>
> - pip安装运行
>
>   ```
>   pip install Pandora-ChatGPT
>   pandora
>   ```
>
> - 编译运行
>
>   ```
>   pip install .
>   pandora
>   ```
>
> - Docker Hub运行
>
>   ```
>   docker pull pengzhile/pandora
>   docker run -it --rm pengzhile/pandora
>   ```
>
> - Docker编译运行
>
>   ```
>   docker build -t pandora .
>   docker run -it --rm pandora
>   ```
>
> - 输入用户名密码登录即可，登录密码理论上不显示出来，莫慌。
>
> - 简单而粗暴，不失优雅。

更多请参考原作者，同样附上成品展示：

![image-20230223185809074](/images/image-20230223185809074.png)

三个项目都使用了Access Token

# 关于 Access Token

- 使用`Access Token`方式登录，可以无代理直连。
- 通常使用`Google`或`Microsoft`账号登录`ChatGPT`的人会用到
- 首先正常登录`ChatGPT`，不管是账号密码，还是`Google`或是`Microsoft`。
- 登录成功到聊天页面后打开：`https://chat.openai.com/api/auth/session`。
- 其中`accessToken`字段的那一长串内容即是`Access Token`。
- `Access Token`可以复制保存，其有效期目前为`8小时`。
- 不要泄露你的`Access Token`，使用它可以操纵你的账号。
