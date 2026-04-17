# 1Cat Tunnel Client

这个 npm 包用于分发和启动预编译的 `1Cat Tunnel` 客户端二进制。

它不会发布 Go 源码，npm 包内会自带对应平台的预编译程序。

## 安装要求

- 已安装 Node.js 和 npm
- 客户机可以主动访问服务端控制地址
- 服务端已经部署完成，并且你能在控制台签发节点专属 `bootstrap token`

当前支持的平台：

- Linux `amd64`
- Windows `amd64`
- macOS `arm64`
- macOS `amd64`

## 安装

在客户机上执行：

```bash
npm install -g 1cat-tunnel-client
```

安装完成后启动客户端：

```bash
hash -r
1cat-tunnel-client
```

如果你刚安装完仍然提示 `command not found`，通常只是当前终端还没刷新命令缓存。

可以任选一种方式：

- 先执行一次 `hash -r`
- 重新打开一个新的终端窗口

说明：

- 使用 `nvm` 的机器不要加 `sudo`
- 普通用户直接执行 `npm install -g ...` 即可

## 一条龙安装流程

如果你是第一次在客户机上安装，可以直接照下面做：

### 1. 在服务端控制台签发 token

先在服务端 Web 控制台为这个节点创建专属 `bootstrap token`。

### 2. 在客户机安装客户端

```bash
npm install -g 1cat-tunnel-client
hash -r
```

### 3. 启动客户端

```bash
1cat-tunnel-client
```

### 4. 按向导完成配置

依次完成：

1. 粘贴 `bootstrap token`
2. 选择要开放的预设，例如 `SSH` 或 `RDP`
3. 确认本地地址
4. 保存配置并启动

完成后，客户端会自动连接到服务端。

## 首次配置

第一次运行时，如果本地还没有现成配置，程序会自动进入首启向导。

你只需要按顺序做这几步：

1. 先在服务端控制台为这个节点签发一个专属 `bootstrap token`
2. 在客户机运行 `1cat-tunnel-client`
3. 粘贴 `bootstrap token`
4. 选择要开放的预设，例如 `SSH` 或 `RDP`
5. 确认本地地址
6. 保存配置并启动

完成后，客户端会自动连接你的服务端。

后续再次运行 `1cat-tunnel-client` 时，会直接复用本地配置，不需要重新输入 token。

## 配置和日志位置

- Linux: `${XDG_CONFIG_HOME:-~/.config}/1cat-tunnel`
- Windows: `%APPDATA%\1cat-tunnel`
- macOS: `~/Library/Application Support/1cat-tunnel`

这个目录里会保存：

- 客户端配置文件
- 运行日志
- 当前用户可执行的客户端二进制副本

## 典型使用流程

### 1. 先准备服务端

确保服务端已经可以正常打开 Web 控制台，并且客户端机器能够访问服务端控制地址。

### 2. 在服务端签发节点 token

登录服务端控制台后，为客户机创建节点，并复制它的专属 `bootstrap token`。

### 3. 在客户机安装并启动

```bash
npm install -g 1cat-tunnel-client
1cat-tunnel-client
```

### 4. 粘贴 token 并完成向导

客户端会自动解析：

- 服务端地址
- 节点名
- 节点访问凭据

你只需要继续选择预设并确认本地端口即可。

## 发布内容

npm 包和 GitHub Release 都会附带这些文件：

- `tunnel-client-linux-amd64`
- `tunnel-client-windows-amd64.exe`
- `tunnel-client-darwin-arm64`
- `tunnel-client-darwin-amd64`
- `checksums.txt`

`npm install` 时会优先使用 npm 包内自带的二进制；如果包内资产不存在，才会回退到 GitHub Release 下载。

## 常见问题

### 1. 安装时报下载失败

通常是当前网络阻止了 npm registry 或 GitHub Release。

可以先检查：

- 客户机是否能访问 GitHub
- 网络代理是否允许下载 Release 资产
- 当前平台是否在支持列表内

如果客户现场网络限制较多，也可以改用已经打好的离线交付包。

### 1.1 命令不存在或没有生效

如果你已经安装完成，但终端提示：

- `command not found: 1cat-tunnel-client`

请先执行：

```bash
hash -r
which 1cat-tunnel-client
```

如果 `which` 仍然没有输出，重新打开一个新的终端窗口再试。

### 1.2 不要用 sudo

如果你的 Node.js 是通过 `nvm` 安装的，不要用：

```bash
sudo npm install -g 1cat-tunnel-client
```

否则 `sudo` 进入的环境通常拿不到你的 `nvm` 路径。

### 2. 运行后没有进入向导

说明当前用户目录里已经存在旧配置。可以先检查上面的配置目录。

### 3. 为什么仓库里没有源码

这个仓库只用于客户端二进制分发，不用于公开源码托管。

## 开发联调

如果你是在本地开发 npm 包，而不是在客户机安装，可以先生成 release assets：

```bash
sh ./scripts/build-release-assets.sh
```

然后在项目根目录执行：

```bash
cd npm/client
npm test
npm pack
```
