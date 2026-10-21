<div align="center">

<h1>Creo 工作目录清理工具</h1>

<p><b>识别 Creo 版本号，保留最新文件，安全释放磁盘空间</b></p>

<p>
  <img src="https://img.shields.io/badge/version-1.0.1-2563EB?style=flat-square" alt="Version">
  <img src="https://img.shields.io/badge/platform-Windows%2010%20%7C%2011-0078D4?style=flat-square" alt="Platform">
  <img src="https://img.shields.io/badge/python-3.10%2B-3776AB?style=flat-square" alt="Python">
  <img src="https://img.shields.io/badge/UI-Tkinter-0F172A?style=flat-square" alt="UI">
</p>

一个面向 Creo 工作目录的 Windows 图形化清理工具：自动识别 `文件名.扩展名.版本号`，按规则保留最新版本，清理旧版本、日志和导出文件。使用 Python + Tkinter 编写，已打包为无需安装 Python 的 EXE。

**作者：大肥鱼1017**

<p>
  <a href="../../releases/latest"><b>⬇️ 前往 Releases 下载</b></a>
  &nbsp;·&nbsp;
  <a href="#功能">功能</a>
  &nbsp;·&nbsp;
  <a href="#清理规则">清理规则</a>
  &nbsp;·&nbsp;
  <a href="#常见问题">常见问题</a>
  &nbsp;·&nbsp;
  <a href="#从源码构建">从源码构建</a>
</p>

</div>

---

## 界面预览

| 主界面 | 格式选择 |
| :---: | :---: |
| ![主界面](docs/screenshot_main.png) | ![格式选择](docs/screenshot_formats.png) |

## 功能

- 📁 **目录扫描**：输入或浏览选择 Creo 工作目录，自动识别文件与版本号
- ✅ **格式勾选**：按格式选择要清理的文件，默认勾选 Creo 原生格式
- 🔢 **版本保留**：保留最新 N 版（默认 3 版，可设置 1–999）
- 🧷 **初版保护**：可选保留初版 `.1`；若没有 `.1`，自动保护当前最早版本
- 🌳 **子目录扫描**：可选递归处理子文件夹，默认只处理当前目录
- 🗒️ **普通文件控制**：无版本号文件默认保留，勾选后才清理
- 👀 **完整预览**：删除前列出每个文件的操作、版本、大小、时间和原因
- ♻️ **回收站优先**：默认移入 Windows 回收站，可恢复；永久删除需二次确认
- 🛡️ **删除前校验**：文件在预览后发生变化、被替换或已删除时自动跳过
- 📤 **清单导出**：可将预览结果导出为 CSV
- 📝 **操作日志**：详细日志保存在本机，便于追溯

## 下载

请前往仓库的 **Releases** 页面下载最新版本：

| 版本 | 文件 | 适合场景 |
| --- | --- | --- |
| 单文件版 | `CreoCleanUp.exe` | 只有一个 exe，方便拷贝和直接运行 |
| 便携版 | `CreoCleanUp_v1.0.1_portable.zip` | 解压即用，兼容性更好，推荐放桌面/U 盘 |

> **单文件版说明**：PyInstaller 单文件程序启动时会在当前工作目录临时解压运行库（`_MEI...`），退出后自动删除。请将 exe 放在有写入权限的本地文件夹中直接双击；若提示无法创建临时目录，请使用便携版。  
> **便携版说明**：需保留整个文件夹（包含 `_internal`），不能只拷贝 exe。

## 快速开始

1. 运行 `CreoCleanUp.exe`。
2. 选择 Creo 工作目录（可粘贴路径后按回车）。
3. 点击 **扫描目录**。
4. 点击 **文件格式**，勾选要清理的格式，点击 **应用选择**。
5. 输入每个文件要保留的最新版数，默认 `3`。
6. 根据需要勾选 **保留初版 / 最早版**。
7. 在右侧预览中确认结果，点击 **执行清理**。
8. 二次确认后执行。默认删除到回收站。

## 支持的格式

| 分类 | 格式 |
| --- | --- |
| Creo 原生 | `.prt` `.asm` `.drw` `.frm` `.sec` `.lay` `.mfg` |
| 日志 / 交换 | `.log` `.stp` `.step` `.igs` `.iges` `.xml` |
| SolidWorks | `.sldprt` `.sldasm` `.slddrw` |
| 其他 CAD / 网格 | `.dwg` `.dxf` `.stl` `.obj` `.3mf` `.wrl` `.x_t` `.x_b` `.sat` `.jt` `.3dxml` `.neu` `.xpr` `.xas` `.g` `.c` `.tbl` `.idx` `.txt` `.pdf` |
| 自定义 | 点击“＋格式”可添加任意扩展名 |

扫描时会自动发现其他带版本号后缀的格式，并显示在“自动识别的其他格式”中。

## 清理规则

Creo 通常把同一模型的不同版本保存为：

```text
housing.prt.1
housing.prt.2
housing.prt.3
housing.prt.4
housing.prt.5
housing.prt.6
```

程序按版本号数字从大到小取 N 个，**不要求版本号连续**。

### 规则示例

| 现有版本 | 保留设置 | 保留 | 删除 |
| --- | --- | --- | --- |
| `.1`～`.6` | 最新 3 版 | `.4` `.5` `.6` | `.1` `.2` `.3` |
| `.1`～`.6` | 最新 3 版 + 初版 | `.1` `.4` `.5` `.6` | `.2` `.3` |
| 只有 `.1` | 最新 1 版，不勾选初版 | `.1` | 无 |
| `.2` `.6` `.8` `.9` | 最新 2 版 + 初版 | `.2` `.8` `.9` | `.6` |
| `.2` `.6` `.8` `.9` | 最新 2 版，不勾选初版 | `.8` `.9` | `.2` `.6` |

### 初版 / 最早版

- 勾选“保留初版 / 最早版”后，`.1` 会被额外保留，不占用“最新 N 版”的名额。
- 如果 `.1` 不存在（例如版本从 `.2` 开始），程序会保护**当前实际存在的最早版本**。
- 只有一个 `.1` 时，它本身就在“最新 1 版”中，即使不勾选初版也不会被误删。
- 初版保护按“父目录 + 文件名 + 扩展名”分组，不同文件夹中的同名文件互不影响。

### 无版本号文件

普通 `part.stp`、`drawing.xml`、`readme.log` 等没有 `.数字` 后缀的文件，默认只显示、不删除。勾选 **同时清理所选格式的无版本号文件** 后，才会删除它们。

## 安全设计

- 扫描阶段不会修改任何文件。
- 执行前必须先生成预览并二次确认。
- 默认删除到 Windows 回收站；永久删除使用红色警告。
- 删除前校验文件路径、大小、修改时间和文件索引；预览后发生变化的文件会跳过。
- 不跟随符号链接、快捷方式或目录联接（junction），也不删除目录本身。
- 被 Creo 或其他程序占用的文件会保留，并在结果和日志中说明。
- 网络共享 / 映射网络驱动器不支持回收站，程序会阻止“移入回收站”；如确需清理，只能手动选择永久删除，建议先备份。
- 清理前建议关闭 Creo，并先在测试目录试运行。

## 配置文件与日志

| 内容 | 路径 |
| --- | --- |
| 配置 | `%APPDATA%\CreoCleanUp\settings.json` |
| 日志 | `%LOCALAPPDATA%\CreoCleanUp\logs\cleanup_*.log` |

如果日志目录不可写，程序会回退到系统临时目录或当前目录下的 `.creo_cleanup_logs`。  
设置 `CREO_CLEANUP_DATA_DIR` 环境变量可以重定向配置和日志目录。

## 常见问题

<details>
<summary><b>为什么勾选“保留初版”后，连 .2 也被保留了？</b></summary>

如果当前模型没有 `.1` 版（例如版本从 `.2` 开始），程序会把实际存在的最早版本视为“初版”的保护对象。这样可以避免“勾选了保留初版，但最早的历史版本仍被删除”的情况。

</details>

<details>
<summary><b>只有一个 .1 文件，不勾选初版、保留 1 版，会删除它吗？</b></summary>

不会。`.1` 同时属于“最新 1 版”，会被正常保留。

</details>

<details>
<summary><b>版本号不连续，如何计算最新 N 版？</b></summary>

按实际版本号数字排序，取最大的 N 个。例如 `.2 .6 .8 .9` 保留 2 版，就是 `.8` 和 `.9`，不要求 `.7` 存在。

</details>

<details>
<summary><b>单文件版提示无法创建临时目录怎么办？</b></summary>

这是 PyInstaller 在部分中文用户名/受限目录环境下的临时目录问题。请改用便携版，或将单文件 exe 放到有写入权限的本地文件夹后直接双击运行。

</details>

<details>
<summary><b>网络目录为什么不能删除到回收站？</b></summary>

Windows 回收站不支持网络共享和映射网络驱动器。程序会阻止该操作，避免文件被不可恢复地删除。如确需清理，请先备份，再手动取消回收站选项并选择永久删除。

</details>

<details>
<summary><b>为什么杀毒软件会报警？</b></summary>

PyInstaller 打包的程序偶尔会被杀毒软件误报。可以添加信任，或使用便携版。程序只使用 Python 标准库，不包含网络上传逻辑。

</details>

## 从源码运行

要求：Windows 10/11、Python 3.10+（推荐 3.12），Tkinter 随官方 Python 安装。

```powershell
python main.py
python main.py "D:\CAD\Project"
```

运行测试：

```powershell
python -m unittest discover -s tests -v
```

## 从源码构建

安装 PyInstaller 并构建单文件版：

```powershell
powershell -ExecutionPolicy Bypass -File build.ps1
```

构建便携文件夹版和 zip：

```powershell
powershell -ExecutionPolicy Bypass -File build_portable.ps1
```

构建产物：

```text
release/
├── CreoCleanUp.exe
├── CreoCleanUp_v1.0.1_portable.zip
└── portable/CreoCleanUp/
```

> 单文件构建使用 `--runtime-tmpdir "."`，让运行库解压到当前工作目录，以规避部分中文用户名环境下 PyInstaller 默认临时目录创建失败的问题。

## 项目结构

```text
core.py                 扫描、版本解析、清理计划、删除与回收站
ui.py                   Tkinter 图形界面
main.py                 程序入口
assets/                 图标资源
docs/                   README 截图
tests/test_core.py      核心逻辑单元测试
build.ps1               单文件版打包脚本
build_portable.ps1      便携版打包脚本
version_info.txt        EXE 版本信息
tools/                  开发辅助脚本
```

## 更新日志

### v1.0.1

- 修复“无 .1 版本时，勾选保留初版但最早版本仍被删除”的问题；现在会保护当前最早版本。
- 增加“只有 .1”“版本号不连续”等边界场景测试。
- 更新界面说明、帮助文档和 README。

### v1.0.0

- 首个版本：目录扫描、格式选择、版本保留、初版保护、预览、回收站/永久删除、CSV 导出和日志。

## 作者

**大肥鱼1017**

如果这个工具对你有帮助，欢迎在仓库点一个 ⭐ Star，或提交 Issue 反馈使用中遇到的问题。

> 本工具与 PTC 无关联。Creo 是 PTC 的商标，仅用于说明兼容的文件格式。
