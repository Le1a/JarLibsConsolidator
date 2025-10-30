## JarLibsConsolidator

一键收集并合并项目中的 JAR 依赖，统一输出到 `all-in-one` 目录，并自动添加为项目库，挂载到所有模块。

![Kotlin](https://img.shields.io/badge/Kotlin-1.9.25-7F52FF?logo=kotlin) ![Gradle](https://img.shields.io/badge/Gradle-8.x-02303A?logo=gradle) ![IntelliJ%20Platform](https://img.shields.io/badge/IntelliJ%20Platform-241--251.*-000?logo=intellijidea) ![JDK](https://img.shields.io/badge/JDK-17-5382A1)

### 📺 演示视频

<div align="center">
  <a href="https://www.youtube.com/watch?v=vE4H3-4ami0">
    <img src="https://img.youtube.com/vi/vE4H3-4ami0/maxresdefault.jpg" alt="JarLibsConsolidator Plugin Demo" width="560" height="315" style="border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  </a>
  <br>
  <p><em>🎬 点击播放按钮观看完整演示 | <a href="https://www.youtube.com/watch?v=vE4H3-4ami0">在新窗口打开</a></em></p>
</div>

### 背景与动机

在进行代码审计工作时，我们经常需要分析已编译打包的 JAR 源码项目。然而，IntelliJ IDEA 往往无法自动识别所有的依赖关系，特别是在分析国产开源项目时更为明显。过去我一直使用以下命令来手动收集依赖：

```bash
cp `find ./ -name "*.jar"` ./all-in-one
```

但每次操作都颇为繁琐。

为了提高工作效率，我开发了这个插件，让依赖收集变得简单高效——只需右键点击，即可完成所有 JAR 文件的收集、整理和项目库配置。

### 特性
- **一键操作**：右键项目 → 选择"**一键添加依赖**"。
- **智能扫描**：递归扫描 `.jar`，跳过常见目录（如 `node_modules`、`target`、`build`、`.gradle`、`.mvn` 等）。
- **重名处理**：自动对同名 jar 加后缀去重（如 `x.jar` → `x_2.jar`）。
- **统一管理**：复制到 `all-in-one/`，创建项目级库 `all-in-one` 并添加至所有模块依赖。
- **版本兼容**：适配 2024.1–2025.1+（build `241`–`251.*`）线程模型与 API。

### 前置依赖

由于 Gradle 分发包体积较大（130MB+），需要手动下载并放置到项目根目录：

```bash
# 下载 Gradle 8.11.1 分发包
wget https://mirrors.cloud.tencent.com/gradle/gradle-8.11.1-bin.zip

# 或者使用 curl
curl -O https://mirrors.cloud.tencent.com/gradle/gradle-8.11.1-bin.zip

# 确保文件位于项目根目录
ls gradle-8.11.1-bin.zip
```

**注意**：该文件已被 `.gitignore` 排除，不会被提交到版本控制中。

### 快速上手
1) **插件已上传至JetBrains Marketplace，可直接在IDEA插件市场搜索JarLibsConsolidator进行安装**

2) 本地运行（开发/体验）

```bash
./gradlew runIde
```

3) 打包安装（生成可安装的 zip）

```bash
./gradlew buildPlugin
# 产物：build/distributions/JarLibsConsolidator-<version>.zip
# IDE 中安装：Settings/Preferences → Plugins → ⚙ → Install Plugin from Disk…
```
或者直接用Releases里我打包好的jar包，在IDEA插件选择本地磁盘安装即可。

### 使用
- 在 Project 视图中右键项目根目录或任意目录 → 选择“**一键添加依赖**”。
- 若 `all-in-one/` 已存在，会提示是否删除并重建。
- 完成后可在项目结构的 `Libraries` 看到 `all-in-one`，并已挂载至所有模块。

### 工作原理（简述）
1. 遍历工程目录收集所有 `.jar` 文件（跳过常见无关目录）。
2. 复制到 `all-in-one/`，处理重名冲突。
3. 创建/刷新项目库 `all-in-one`，将 jar 作为 `CLASSES` 根添加，并依附到所有模块。

### 兼容性与要求
- **IDE**：IntelliJ IDEA 2024.1 – 2025.1+（build `241`–`251.*`）
- **JDK**：17
- **运行时插件**：`com.intellij.java`（已通过平台打包）

### 常见问答
- **会修改源码吗？** 不会，仅复制 jar 并写入项目库配置。
- **扫描很慢怎么办？** 建议在项目根执行，插件已默认跳过体量较大的常见目录；也可在更小的子目录执行。

### 开发
```bash
# 验证插件
./gradlew verifyPlugin

# 本地运行沙箱 IDE
./gradlew runIde

# 构建可分发包
./gradlew buildPlugin
```

