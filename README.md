# MD编辑器 — 安卓版 Markdown 文本编辑器

[![Android](https://img.shields.io/badge/Android-7.0%2B-green?logo=android)](https://developer.android.com)
[![Kotlin](https://img.shields.io/badge/Kotlin-1.9-7F52FF?logo=kotlin)](https://kotlinlang.org)
[![Compose](https://img.shields.io/badge/Jetpack_Comose-latest-4285F4?logo=jetpackcompose)](https://developer.android.com/jetpack/compose)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

一款**仿 iOS 风格**的 Android Markdown 编辑器，专注于标题与有序列表的高效编辑。支持完整的文件操作、实时预览、查找替换、自动编号等功能。界面简洁，支持深色/浅色双主题。

## 📸 界面预览

- **浅色模式** — iOS 风格浅色背景 (`#F2F2F7`)，白色卡片
- **深色模式** — 纯黑背景 (`#000000`)，深灰卡片 (`#1C1C1E`)
- **格式栏** — 紧贴屏幕顶部，一键切换正文/标题 H1–H5/有序列表/缩进/加粗/文字颜色
- **预览模式** — WebView 渲染 Markdown，支持标题、列表、行内代码、代码块、引用、链接

## ✨ 功能特性

### 核心编辑

- **标题快捷操作** — 一键将当前行设为 H1 ~ H5 标题，或恢复为正文
- **有序列表与缩进** — 一键添加/移除有序列表，左右箭头控制缩进级别（以 2 空格为单位）
- **选区批量操作** — 选中多行后，可批量应用标题/列表/缩进
- **加粗与彩色文字** — 选中文本后一键添加 `**粗体**` 或 `[c=#RRGGBB]彩色文字[/c]` 标记
- **回车自动编号** — 有序列表换行时自动延续下一编号

### 文件操作

- **新建** — 新建 MD 文档
- **打开** — 通过系统 SAF 文件选择器打开文本文件
- **保存 / 另存为** — 覆盖保存或选择新路径，支持持久化 URI 权限
- **导出** — 导出为 Markdown、纯文本 (TXT)、HTML 三种格式
- **最近打开** — 记录最近 20 个打开的文件，一键快速加载

### 预览模式

- WebView 实时渲染 Markdown，自定义 iOS 风格 CSS
- 支持标题、列表、代码块、引用、链接等常见语法
- JavaScript 已禁用，安全无 XSS 风险

### 辅助功能

- **查找与替换** — 支持单个替换和全部替换，显示匹配计数与当前定位
- **字数统计** — 字符数（含/不含空格）、单词数、行数
- **行号显示** — 编辑器左侧显示行号，可开关
- **重新编号** — 一键重排全文标题层级编号与有序列表编号
- **撤销 / 重做** — 利用 EditText UndoManager (API 31+)

### 个性化设置

- 字体大小调节 (14sp ~ 22sp)
- 浅色 / 深色 / 跟随系统 三档主题
- 自动重新编号开关（标题 / 列表可独立控制）
- 毛玻璃效果开关（API 31+）

## 🛠️ 技术栈

| 组件 | 技术选型 |
|------|----------|
| 语言 | Kotlin |
| UI 框架 | Jetpack Compose (自定义主题) |
| 编辑器 | 原生 EditText (AndroidView 包装) |
| Markdown 解析 | 手写 LineParser（编辑模式行类型判断） |
| 预览渲染 | WebView + 自写 CSS（无第三方 Markdown 渲染库依赖） |
| 文件存储 | SAF (Storage Access Framework) + 持久化 URI |
| 持久化 | DataStore Preferences |
| 异步 | Kotlin Coroutines + StateFlow |
| 最低 SDK | API 24 (Android 7.0) |

## 📂 项目结构

```
MDEditor/
├── app/
│   ├── build.gradle.kts              # 应用构建配置与依赖
│   ├── proguard-rules.pro            # 混淆规则
│   └── src/
│       ├── main/
│       │   ├── AndroidManifest.xml
│       │   ├── res/values/
│       │   │   ├── strings.xml       # 字符串资源
│       │   │   └── themes.xml        # 主题资源
│       │   └── java/com/mdeditor/app/
│       │       ├── MainActivity.kt              # 主 Activity（所有 Compose UI）
│       │       ├── viewmodel/
│       │       │   └── EditorViewModel.kt       # 编辑器状态与文本操作
│       │       ├── ui/
│       │       │   ├── theme/
│       │       │   │   ├── Color.kt             # iOS 风格色板
│       │       │   │   ├── Type.kt              # 字体排版定义
│       │       │   │   └── Theme.kt             # Compose 主题
│       │       │   ├── components/
│       │       │   │   ├── ActionSheet.kt       # iOS 风格底部操作表
│       │       │   │   ├── ColorPickerSheet.kt  # 颜色选择器弹窗
│       │       │   │   ├── FormatBar.kt         # 格式栏（标题/列表/缩进/颜色）
│       │       │   │   ├── MarkdownPreview.kt   # Markdown 预览 (WebView)
│       │       │   │   ├── SearchReplaceBar.kt  # 查找与替换栏
│       │       │   │   └── Toolbar.kt           # 功能工具栏
│       │       │   └── screens/
│       │       │       └── SettingsScreen.kt    # 设置页面
│       │       └── util/
│       │           ├── AppSettings.kt           # DataStore 设置管理
│       │           ├── BlurUtils.kt             # 毛玻璃模糊工具
│       │           └── LineParser.kt            # 行类型解析器（核心）
│       └── test/java/com/mdeditor/app/util/
│           └── LineParserTest.kt                # LineParser 单元测试 (27 用例)
├── build.gradle.kts                   # 根构建配置
├── settings.gradle.kts                # 项目设置
├── gradle.properties                  # Gradle 属性
└── gradlew / gradlew.bat              # Gradle 包装器
```

## 🚀 快速开始

### 环境要求

- Android Studio Hedgehog (2023.1.1) 或更高版本
- JDK 17
- Android SDK 34
- Gradle 8.7+（使用项目自带的 `gradlew` 即可）

### 构建与运行

```bash
# 克隆仓库
git clone https://github.com/yourusername/MDEditor.git
cd MDEditor

# 使用 Gradle 包装器构建 debug APK
./gradlew assembleDebug          # macOS / Linux
gradlew.bat assembleDebug        # Windows

# 安装到已连接的设备
./gradlew installDebug
```

也可以在 Android Studio 中直接打开项目，选择 `Run > Run 'app'` 编译运行。

### 运行单元测试

```bash
./gradlew testDebugUnitTest
```

## 📖 使用说明

### Markdown 语法支持

| 语法 | 说明 | 快捷操作 |
|------|------|----------|
| `# 标题` ~ `##### 标题` | H1 ~ H5 标题 | 点击格式栏 H1–H5 按钮 |
| `1. 项目` / `  2. 子项` | 有序列表（2 空格缩进） | 点击 `1.` 按钮，左右箭头调整缩进 |
| `**文字**` | 加粗 | 选中文字后点击 `B` 按钮 |
| `[c=#RRGGBB]文字[/c]` | 彩色文字 | 点击色块选择颜色 |

### 格式栏操作

| 按钮 | 功能 |
|------|------|
| **H1 – H5** | 切换当前行/选区为对应标题级别 |
| **B** | 选中文字加粗（再次点击切换还原） |
| **← / →** | 标题升降级 / 列表缩进增减 |
| **1.** | 切换有序列表 |
| **色块** | 为选中文字添加颜色标记 |

### 文件管理

- **左侧菜单 (☰)** — 新建、打开、保存、关闭、最近打开、导出、设置、关于
- **右侧菜单 (⋮)** — 编辑/预览切换、查找替换、字数统计、全选/复制/剪切/粘贴、重新编号、行号开关

## ⚠️ 注意事项

1. **文件权限** — 使用 SAF 访问文件，部分设备重启后持久化权限可能失效，重新打开会提示用户重新选择
2. **WebView** — 预览模式依赖系统 WebView，部分 Android 定制系统可能需要更新 WebView
3. **毛玻璃效果** — 仅在 API 31 (Android 12) 及以上版本生效，低版本自动降级为半透明遮罩
4. **撤销重做** — 利用 EditText 内置 UndoManager，API 31 以下版本功能受限

## 📝 许可证

[MIT License](LICENSE)

## 👤 作者

帅气的锅巴

---

> 路虽远，行则将至；事虽难，做则必成。
