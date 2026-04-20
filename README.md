# pi-weixinbot

微信机器人 extension for pi，支持扫码登录和消息收发。

参考项目：[Tencent/openclaw-weixin](https://github.com/Tencent/openclaw-weixin)

## 功能特性

- 🔐 **扫码登录** - 使用微信扫码授权，无需手动输入 token
- 💬 **消息接收** - 自动接收微信消息并转发给 AI 处理
- 📤 **消息发送** - AI 回复自动发送回微信
- 👤 **多账户支持** - 支持管理多个微信账户
- 🔄 **自动重连** - 重启后自动恢复连接

## 安装

### 方式一：作为 pi package 安装（推荐）

```bash
pi pkg install pi-weixinbot
```

### 方式二：手动安装

1. 克隆仓库到本地：
```bash
git clone https://github.com/huang-x-h/pi-weixinbot.git
```

2. 在项目目录安装依赖：
```bash
cd pi-weixinbot
npm install
```

3. 链接到 pi 扩展目录：
```bash
# 创建符号链接或复制到扩展目录
ln -s /path/to/pi-weixinbot ~/.pi/agent/extensions/pi-weixinbot
```

## 使用方法

### 1. 登录微信

启动 pi 后，使用命令登录微信：

```
/weixin-login
```

或者使用工具：

```
使用 weixin_login 工具登录微信
```

登录过程：
1. 系统会输出一个二维码链接
2. 使用微信扫描二维码
3. 在手机上确认授权
4. 登录成功后会自动开始接收消息

### 2. 发送消息

当收到微信消息时，AI 会自动处理并回复。

也可以手动发送消息：

```
使用 weixin_send 工具发送消息
参数：text="你好，这是一条测试消息"
```

### 3. 查看状态

```
/weixin-status
```

或者：

```
使用 weixin_status 工具
```

### 4. 退出登录

```
使用 weixin_logout 工具
```

## 工具列表

| 工具名 | 说明 | 参数 |
|--------|------|------|
| `weixin_login` | 扫码登录微信 | 无 |
| `weixin_logout` | 退出微信登录 | `accountId` (可选) |
| `weixin_send` | 发送文本消息 | `text`, `to` (可选) |
| `weixin_status` | 查看连接状态 | 无 |

## 命令列表

| 命令 | 说明 |
|------|------|
| `/weixin-login` | 扫码登录微信 |
| `/weixin-status` | 查看连接状态 |

## 工作原理

本 extension 参考了 [Tencent/openclaw-weixin](https://github.com/Tencent/openclaw-weixin) 项目，实现以下功能：

1. **扫码登录**：通过微信开放平台的 ilink API 获取二维码并轮询登录状态
2. **消息接收**：使用长轮询（long-polling）方式从微信服务器获取新消息
3. **消息发送**：通过 sendMessage API 发送文本消息到微信

### API 端点

- 二维码获取: `GET https://ilinkai.weixin.qq.com/ilink/bot/get_bot_qrcode`
- 登录状态: `GET https://ilinkai.weixin.qq.com/ilink/bot/get_qrcode_status`
- 消息获取: `POST https://ilinkai.weixin.qq.com/ilink/bot/getupdates`
- 消息发送: `POST https://ilinkai.weixin.qq.com/ilink/bot/sendmessage`

## 数据存储

登录信息存储在 `~/.pi/agent/weixin/` 目录：

```
~/.pi/agent/weixin/
├── accounts.json      # 账户索引
├── accounts/          # 各账户数据
│   ├── xxx.json       # 具体账户信息（token 等）
└── config.json        # 全局配置
```

## 注意事项

1. **安全提示**：存储的 token 文件权限为 600，请确保系统安全
2. **会话过期**：微信登录会话可能过期，如果出现错误请重新登录
3. **多设备限制**：微信可能限制同一账号的并发连接数

## 故障排除

### 登录失败

- 检查网络连接
- 确认微信账号状态正常
- 尝试重新生成二维码

### 消息接收不到

- 检查是否已登录成功
- 确认 AI 正在运行
- 查看日志中的错误信息

### Session 过期

- 使用 `/weixin-login` 重新登录

## License

MIT
