# 1Cat Tunnel Client

这个 npm 包用于分发和启动预编译的 `1Cat Tunnel` 客户端二进制。

它不会发布 Go 源码，客户端安装时会从 GitHub Release 下载对应平台的预编译程序。

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
1cat-tunnel-client
```

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

GitHub Release 会附带这些文件：

- `tunnel-client-linux-amd64`
- `tunnel-client-windows-amd64.exe`
- `tunnel-client-darwin-arm64`
- `tunnel-client-darwin-amd64`
- `checksums.txt`

`npm install` 时会自动下载并校验这些文件。

## 常见问题

### 1. 安装时报下载失败

通常是客户机当前无法访问 GitHub Release。

可以先检查：

- 客户机是否能访问 GitHub
- 网络代理是否允许下载 Release 资产
- 当前平台是否在支持列表内

如果客户现场网络限制较多，也可以改用已经打好的离线交付包。

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
