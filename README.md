# WebInvInspector v2.0.0

Minecraft BDS 物品查看器，支持实时查看玩家背包、物品变更记录等功能。

## 功能特性

- 🎮 **实时背包查看** - 查看在线玩家的背包、装备、末影箱
- 📊 **物品变更追踪** - 记录物品获得、丢失、位置变更
- 🔍 **物品搜索** - 按物品ID搜索特定物品的流向
- 📈 **数据统计** - 物品流量统计，服务器经济分析
- 💾 **历史记录** - 查看历史快照，支持多日期查询
- 🖥️ **桌面程序** - 独立exe程序，无需浏览器
- 🔐 **安全认证** - Token认证保护数据安全

## 安装说明

### 1. 服务端插件安装

将 `WebInvInspector.jsc` 文件放入以下目录：
```
plugins/Meow/plugins/WebInvInspector.js
```
或者将Meow文件夹存放入plugins文件夹内

### 2. 桌面程序运行

双击 `WebInvInspector.exe` 启动程序，程序会自动：
- 启动内置Web服务器（端口3001）
- 打开操作界面
- 等待BDS服务器连接

### 3. 获取认证Token

在BDS服务器控制台输入：
```
/webinv token
```
复制显示的Token，在程序界面中输入即可开始使用。

## 使用方法

1. **启动程序** - 运行WebInvInspector.exe
2. **输入Token** - 使用BDS服务器生成的Token
3. **查看数据** - 界面会自动同步并显示服务器数据
4. **功能导航**：
   - **面板** - 查看在线玩家和实时背包
   - **搜索** - 按物品ID搜索流向
   - **统计** - 查看物品流量统计
   - **设置** - 配置插件参数

## 配置文件

插件配置文件位置：`./Meowdata/WebInvInspector/config.json`

```json
{
  "serverId": "server-001",
  "serverName": "我的服务器",
  "webServerUrl": "http://127.0.0.1:3001",
  "token": "自动生成的认证token",
  "snapshotInterval": 300,
  "maxDaysToKeep": 30
}
```

## 系统要求

- **Windows 10/11** (64位)
- **Node.js 16+** (开发环境)
- **LeviLamina/LSE-NodeJS** (BDS服务端)
- **Meow插件系统** v1.0+

## 命令列表

- `/webinv status` - 查看插件状态
- `/webinv reload` - 重新加载配置
- `/webinv token` - 显示认证Token

## 技术架构

- **后端**: LeviLamina + Meow插件系统
- **前端**: Electron + Express + SQLite
- **通信**: HTTP Push模型 + Token认证
- **存储**: SQLite数据库 + 文件缓存

## 常见问题

**Q: 程序无法启动？**
A: 确保端口3001未被占用，或检查防火墙设置。

**Q: 无法连接到服务器？**
A: 检查Token是否正确，确保BDS服务器正在运行。

**Q: 数据不显示？**
A: 等待几分钟让数据同步，或使用 `/webinv reload` 命令。

## 支持与反馈

- 作者：伊希娅
- 发布平台：Minebbs
- 更新日志：见插件内版本信息

---

**注意**: 本插件仅适用于合法的服务器管理，请勿用于违规用途。
