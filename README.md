# 音乐解锁插件 tool.unlock

**适用于 LNH-MusicTag** 的音乐解锁插件：将 QQ音乐（mflac / mgg / qmc）、网易云（ncm）、酷狗（kgm / vpr）、酷我（kwm）、虾米（xm）等加密音乐文件解密为可播放格式。

纯 Go 实现、零外部依赖，Windows / Linux 双平台，内置网页操作面板。

## 功能特性

- **支持格式**：mflac / mgg / qmc（QQ音乐）、ncm（网易云）、kgm / vpr（酷狗）、kwm（酷我）、xm（虾米）
- **批量扫描**：输入目录一键扫描，自动列出目录下全部加密音乐文件
- **批量解密**：勾选文件后批量解密，逐文件显示成功/失败状态与进度条
- **跳过已解密**：默认跳过已解密的文件（按扩展名判断），避免重复劳动
- **网页面板**：插件自带操作界面，随宿主显示，无需命令行
- **跨平台**：Windows amd64 / Linux amd64 双二进制
- **零外部依赖**：纯 Go 实现，密钥表内嵌，无网络依赖、无运行时依赖

## 安装

### 推荐：后台导入 ZIP 插件

1. 登录 LNH-MusicTag 后台
2. 进入 **插件管理**
3. 点击 **导入 ZIP 插件**，选择插件包 `tool.unlock-v1.1.0.zip`（可从 Releases 页面下载）
4. 导入后启用「音乐解锁」插件

### 手动安装

1. 将插件目录整体放入 LNH-MusicTag 插件目录 `plugins/tool.unlock/`：
   - Windows 示例：`C:\config\plugins\tool.unlock\`
   - Linux 示例：`<数据目录>/plugins/tool.unlock/`
2. 重启 LNH-MusicTag
3. 进入 **设置 → 插件**，确认「音乐解锁」插件已启用

目录内容：

```
tool.unlock/
├── plugin.json          # 插件清单（名称 / 版本 / 入口）
├── panel.html           # 网页操作面板
└── bin/
    ├── unlock-plugin-windows-amd64.exe   # Windows 可执行文件
    └── unlock-plugin-linux-amd64         # Linux 可执行文件
```

## 使用

1. 打开插件的「音乐解锁」面板
2. 输入音乐目录路径（如 `E:\music` 或 `/music`），点击 **扫描**
3. 勾选要解密的文件（默认全选；可取消勾选「跳过已解密」）
4. 点击 **解密选中**，解密完成后在原目录生成同名可播放文件（扩展名变为 `.flac` / `.mp3` 等）

## API

宿主通过 `POST /api/plugins/tool.unlock/run` 调用插件，需携带请求头 `X-Requested-With: Lnh-Music-Tag`。

### 扫描目录

请求：

```json
{
  "op": "scan",
  "dir": "E:\\music",
  "skip_exist": true
}
```

响应：

```json
{
  "files": [
    { "name": "song.mflac", "path": "E:\\music\\song.mflac", "ext": ".mflac", "size": 12345678 }
  ]
}
```

### 解密文件

请求：

```json
{
  "op": "unlock",
  "file": "E:\\music\\song.mflac",
  "skip_exist": true
}
```

响应：

```json
{ "ok": true }
```

## 版本历史

- **v1.1.0** — 初始发布（优化版）：批量扫描/解密、跳过已解密、网页面板、双平台二进制

## 说明

本插件仅用于解密个人合法获取的音乐文件。请遵守所在地区法律法规及平台服务条款，勿用于传播、倒卖或任何商业用途。
