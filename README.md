# WebInvInspector v2.0.6

Minecraft BDS 背包查看与管理工具，支持实时查看、历史回溯、物品流动分析、背包备份与还原。

## 功能特性

- **实时背包查看** - 查看在线玩家的背包、装备、副手、末影箱
- **嵌套容器展示** - 潜影盒、收纳袋内物品支持深层嵌套查看，交互式容器面板
- **物品变更追踪** - 自动记录物品获得与丢失
- **物品流动统计** - 按日期查看物品流入/流出，支持多模式搜索（名称/数量/类型）
- **全服物品流动** - 查看所有玩家当天的物品流动汇总，含按玩家分组明细
- **历史快照** - 查看任意时间点的背包状态，支持多日期、多事件类型筛选
- **背包备份** - 创建命名备份，保存所有玩家当前背包状态
- **背包还原** - 从备份或历史快照还原玩家背包（支持单人/批量还原）
- **远程命令** - 通过Web界面向BDS服务器发送命令
- **Token认证** - 安全的身份验证机制
- **多服务器支持** - 同一前端可管理多个BDS服务器

## 系统架构

```
BDS服务器 (Meow插件)  --HTTP POST-->  Node.js服务器 (端口3001)  <--浏览器-->  Web前端
         |                                    |
   数据采集 & 命令执行                   SQLite存储 & API服务
```

- **BDS插件** (`plugins/Meow/plugins/Web-InvInspector.js`) - 数据采集、快照记录、命令执行、背包还原
- **Node.js服务器** (`web-inventory-inspector/`) - 数据接收、存储、API服务、静态文件托管
- **Web前端** (`public/`) - 纯HTML/CSS/JS，无框架依赖

## 安装说明

### 1. BDS插件

将 `Web-InvInspector.js` 放入 `plugins/Meow/plugins/` 目录。

### 2. Node.js服务器

```bash
cd plugins/Meow/web-inventory-inspector
npm install
npm start
```

或使用 `start.bat` 一键启动（自动安装依赖、创建配置）。

### 3. 获取Token

在BDS控制台输入：
```
/webinv token
```
在浏览器打开 `http://localhost:3001`，输入Token登录。

## 页面导航

| 页面 | 路径 | 功能 |
|------|------|------|
| 面板 | `/dashboard.html` | 服务器状态、在线玩家、历史玩家、快速查包 |
| 背包 | `/inventory.html` | 实时查看玩家背包（含嵌套容器） |
| 快照 | `/snapshots.html` | 历史背包记录，支持还原到任意时间点 |
| 物品流 | `/flow.html` | 物品流动统计，支持全服/单人、多模式搜索 |
| 备份 | `/backups.html` | 创建/管理备份，从备份或历史记录还原背包 |
| 命令 | `/command.html` | 远程执行BDS命令 |

## 配置文件

插件配置：`./Meowdata/WebInvInspector/config.json`

```jsonc
{
  "serverId": "server-001",
  "serverName": "我的服务器",
  "webServerUrl": "http://127.0.0.1:3001",
  "token": "自动生成",
  "maxDaysToKeep": 30,              // 数据保留天数
  "periodicInterval": 10000,        // 定时背包记录间隔（毫秒），默认10秒,若您的存储没那么大建议使用每分钟一次或每5分钟一次
  "features": {
    "enableJoinLeaveRecords": true,  // 记录玩家加入/离开时的背包
    "enablePeriodicSnapshots": true, // 启用定时背包记录
    "enableChangeTracking": true,    // 启用物品变更追踪
    "enableNestedParsing": true      // 启用潜影盒/收纳袋嵌套解析
  }
}
```

Node.js服务器配置：`.env`

```env
PORT=3001
HOST=0.0.0.0
```

## 命令列表

- 本指令已正式移除

## API端点

### BDS推送接口

| 方法 | 路径 | 说明 |
|------|------|------|
| POST | `/push/register` | BDS注册/心跳 |
| POST | `/push/snapshot` | 推送快照数据 |
| POST | `/push/change` | 推送变更数据 |
| POST | `/push/players` | 推送在线玩家 |
| POST | `/push/translations` | 推送翻译表 |
| POST | `/push/dictionary` | 推送字典 |
| POST | `/push/command-result` | 命令执行结果 |
| POST | `/push/sync` | 批量同步 |

### 前端API

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | `/api/players` | 在线玩家列表 |
| GET | `/api/history-players` | 历史玩家列表 |
| GET | `/api/inventory` | 玩家背包数据 |
| GET | `/api/snapshots` | 历史快照列表 |
| GET | `/api/changes` | 物品变更记录 |
| GET | `/api/flow` | 物品流动统计 |
| GET | `/api/translations` | 翻译表 |
| POST | `/api/command` | 提交远程命令 |
| GET | `/api/command-log` | 命令执行日志 |
| POST | `/api/backup/create` | 创建备份 |
| GET | `/api/backup/list` | 备份列表 |
| GET | `/api/backup/detail` | 备份详情 |
| DELETE | `/api/backup/:id` | 删除备份 |
| POST | `/api/backup/restore` | 从备份还原 |
| POST | `/api/snapshot/restore` | 从快照还原 |

## 系统要求

- **Windows 10/11** (64位)
- **Node.js 16+**
- **LeviLamina / LSE-NodeJS**
- **Meow插件系统**

## 依赖

- `express` - Web服务器
- `better-sqlite3` - SQLite数据库
- `compression` - HTTP压缩

## 常见问题

**Q: 无法启动服务器？**
A: 确保端口3001未被占用，检查 `npm install` 是否成功。

**Q: BDS无法连接？**
A: 检查 `config.json` 中 `webServerUrl` 是否正确指向Node.js服务器地址。

**Q: Token在哪里？**
A: 查看 `config.json` 中的 `token` 字段。

**Q: 还原背包失败？**
A: 确保目标玩家在线，且BDS服务器与Node.js服务器连接正常。

**Q: 数据不显示？**
A: 等待BDS心跳同步（约15秒）

## 作者

- 伊希娅 (MeowTeam)
- 发布平台：Minebbs

## 更新日志

### v2.0.6

#### 新增功能
- **物品编辑面板MC风格化** - 编辑面板采用Minecraft原版UI风格，灰色背景、3D凹凸边框
- **药水类型映射扩展** - 新增滞留型药水、喷溅型药水的中文映射

#### Bug修复
- **SNBT解析增强** - 支持JSON风格格式（带引号的键名），修复附魔、自定义名称、Lore解析
- **容器弹窗判断** - 修复背包内物品点击无法打开编辑面板的问题
- **附魔显示** - 修复附魔属性未正确渲染的问题

#### 改进
- **编辑面板字段** - 自定义名称和Lore输入框显示原始SNBT数据（带`§`格式码）
- **placeholder优化** - 显示物品当前名称和Lore预览

---

### v2.0.3

- 嵌套容器展示优化
- 物品变更追踪增强

## 使用说明

### 物品编辑

在背包页面点击物品可打开编辑面板：

- **物品类型** - 可搜索并替换为其他物品
- **数量** - 修改物品堆叠数量（1-64）
- **自定义名称** - 输入框显示原始名称（含`§`格式码），placeholder显示去除格式码后的预览
- **Lore** - 每行一个，输入框显示原始Lore（含`§`格式码）
- **死亡后保留** - 勾选后物品在玩家死亡时不会掉落
- **附魔** - 可添加/删除附魔，支持等级设置

### 容器物品查看

点击潜影盒、收纳袋等容器物品：
- 显示容器内物品列表
- 点击容器内物品显示只读信息面板（不可编辑）
- 支持面包屑导航返回上级容器

### 格式码说明

Minecraft使用`§`作为格式码前缀：
- `§4` - 深红色
- `§6` - 金色
- `§a` - 亮绿色
- `§b` - 青色
- `§l` - 粗体
- `§r` - 重置格式

示例：`§r§4服§6务§g器§a菜§b单` 显示为彩色"服务器菜单"

---

**注意**: 本插件仅适用于合法的服务器管理用途。
