---
title: 运维-Android 6.0 (API 23) 模拟器配置全记录
tags:
  - android
  - 模拟器
categories:
  - IT
  - 运维
abbrlink: 69698ec6
date: 2025-03-14 01:02:10
---
```markdown
# macOS 配置 Android 6.0 模拟器全流程指南（Oracle JDK 17 专版）

---

## 一、环境准备

### 1. 安装 Oracle JDK 17
#### ▶ 安装步骤
1. **下载安装包**  
   访问 [Oracle JDK 17 官方下载页](https://www.oracle.com/java/technologies/downloads/#java17-mac)  
   ✅ 选择 macOS x64 DMG 安装包（需登录 Oracle 账号）

2. **安装 JDK**  
   ```bash
   # 双击下载的 .dmg 文件，按向导完成安装
   # 默认安装路径：
   /Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home
   ```

3. **配置环境变量**  
   ```bash
   # 编辑 Shell 配置文件（~/.zshrc 或 ~/.bash_profile）
   nano ~/.zshrc

   # 添加以下内容
   export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home"
   export PATH="$JAVA_HOME/bin:$PATH"

   # 生效配置
   source ~/.zshrc
   ```

4. **验证安装**  
   ```bash
   java -version
   # 正确输出应包含：
   # Java(TM) SE Runtime Environment (build 17.0.8+11-LTS-211)
   # Java HotSpot(TM) 64-Bit Server VM (build 17.0.8+11-LTS-211, mixed mode, sharing)
   ```

---

### 2. 配置 Android SDK 环境
```bash
# 设置 SDK 默认路径
export ANDROID_SDK_ROOT=$HOME/Library/Android/sdk
export PATH=$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/emulator:$PATH

# 验证路径配置
echo $ANDROID_SDK_ROOT  # 应输出 /Users/你的用户名/Library/Android/sdk
```

---

## 二、Android 6.0 模拟器配置

### 1. 安装必需组件
```bash
# 安装 Android 6.0 (API 23) 系统镜像
sdkmanager "system-images;android-23;google_apis;x86"
# 安装 Android 8.1 (API 27) 系统镜像
sdkmanager "system-images;android-27;google_apis_playstore;x86"

# 
sdkmanager --list
sdkmanager "system-images;android-27;google_apis;arm64-v8a"

# 安装命令行工具
sdkmanager "platform-tools" "emulator"

# 验证组件安装
ls $ANDROID_SDK_ROOT/system-images/android-23/google_apis/x86
# 应包含 system.img、userdata.img 等文件
```

---

### 2. 创建 Nexus 5 模拟器
```bash
# 查看可用设备列表（若无 Nexus_5 则跳过 --device 参数）
avdmanager list device

# 查看AVD列表
avdmanager list avd

# 创建 AVD（自动选择默认设备）
avdmanager create avd --name Nexus5_Android6 --package "system-images;android-23;google_apis;x86"

avdmanager create avd --name Android8_1_apis --package "system-images;android-27;google_apis;x86"
avdmanager create avd --name Android8_1_playstore --package "system-images;android-27;google_apis_playstore;x86"

# 删除 AVD
emulator -delete avd Android6_Emulator

# 手动调整分辨率（Nexus 5 默认 1080x1920）
sed -i '' 's/hw.lcd.height=.*/hw.lcd.height=1920/' ~/.android/avd/Nexus5_Android6.avd/config.ini
sed -i '' 's/hw.lcd.width=.*/hw.lcd.width=1080/' ~/.android/avd/Nexus5_Android6.avd/config.ini
```

---

### 3. 启动与验证模拟器
```bash
# 基础启动命令
emulator -avd Android8_1_apis -no-snapshot-load -writable-system
emulator -avd Android8_1_playstore -no-snapshot-load -memory 2048 -writable-system -gpu swiftshader

# 高级启动参数（可选）
emulator -avd Nexus5_Android6 -no-boot-anim -memory 2048 -gpu swiftshader

# 验证设备连接
adb devices
# 输出示例：emulator-5554   device
```

---

## 三、关键问题解决方案

### 1. `No device found matching --device Nexus_5`
#### 原因分析
- SDK 预设设备名称已更新，`Nexus_5` 不在默认列表中

#### 解决方案
```bash
# 方法 1：不指定设备类型
avdmanager create avd \
  --name Nexus5_Android6 \
  --package "system-images;android-23;google_apis;x86"

# 方法 2：使用相近设备（如 Nexus_5X）
avdmanager create avd \
  --name Nexus5X_Android6 \
  --package "system-images;android-23;google_apis;x86" \
  --device "Nexus_5X"
```

---

### 2. `This tool requires JDK 17 or later`
#### 原因分析
- 系统默认 JDK 为旧版本（如 JDK 1.8）

#### 解决方案
```bash
# 强制指定 Oracle JDK 17 路径
export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home"
export PATH="$JAVA_HOME/bin:$PATH"

# 永久生效配置（添加到 Shell 配置文件）
echo 'export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home"' >> ~/.zshrc
```

---

### 3. 模拟器启动黑屏/卡顿
#### 解决方案
```bash
# 启用软件渲染模式
emulator -avd Nexus5_Android6 -gpu swiftshader

# 安装硬件加速驱动（HAXM）
sdkmanager "extras;intel;Hardware_Accelerated_Execution_Manager"
# 安装完成后重启终端
```

---

## 四、高级操作指南

### 1. 获取 Root 权限
```bash
# 启动时挂载可写系统分区
emulator -avd Nexus5_Android6 -writable-system

# 进入 adb shell 执行 root 操作
adb root          # 重启 adbd 为 root 模式
adb remount       # 挂载系统分区为可读写
adb shell         # 进入设备 Shell
# 验证 root 权限：whoami 应输出 root
```

---

### 2. 自定义硬件参数
```bash
# 修改 AVD 配置文件
nano ~/.android/avd/Nexus5_Android6.avd/config.ini

# 常用参数示例
hw.ramSize=2048       # 内存大小（MB）
disk.dataPartition.size=2G  # 数据分区大小
hw.gpu.enabled=yes    # 强制启用 GPU 加速
```

---

## 五、注意事项

1. **Oracle JDK 商业许可**  
   企业生产环境需遵守 [Oracle 商业许可协议](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)，个人开发/测试可免费使用。

2. **多版本 JDK 管理**  
   ```bash
   # 查看所有已安装 JDK
   /usr/libexec/java_home -V
   # 输出示例：
   # 17.0.8 (x86_64) "Oracle Corporation" - "Java SE 17.0.8" /Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home
   ```

3. **备份与恢复**  
   - AVD 配置文件位置：`~/.android/avd/`
   - 定期备份可快速恢复开发环境

---
