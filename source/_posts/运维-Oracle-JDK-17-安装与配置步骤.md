---
title: 运维-Oracle JDK 17 安装与配置步骤
tags:
  - jdk
  - java
  - oracle
categories:
  - IT
  - 运维
abbrlink: 697b4730
date: 2025-03-14 01:05:43
---


以下是针对 **Oracle JDK 17** 的安装验证与环境配置指南：

---

## **Oracle JDK 17 安装与配置步骤**

### **1. 验证 JDK 安装**
#### Windows
1. 打开命令提示符（CMD）：
   ```cmd
   java -version
   ```
   - 正确输出应显示：
     ```
     java version "17.0.8" 2023-07-18 LTS
     Java(TM) SE Runtime Environment (build 17.0.8+11-LTS-211)
     Java HotSpot(TM) 64-Bit Server VM (build 17.0.8+11-LTS-211, mixed mode, sharing)
     ```

#### macOS/Linux
1. 打开终端：
   ```bash
   java -version
   ```
   - 正确输出应包含 `Java(TM) SE Runtime Environment` 和版本号 `17.x.x`。

---

### **2. 配置环境变量**
#### Windows
1. **设置 `JAVA_HOME`**：
   - 右键 **此电脑** → **属性** → **高级系统设置** → **环境变量** → **系统变量** → **新建**：
     - **变量名**: `JAVA_HOME`
     - **变量值**: `C:\Program Files\Java\jdk-17`（根据实际安装路径调整）

2. **更新 `Path`**：
   - 编辑 **系统变量** → **Path** → **新建**，添加：  
     `%JAVA_HOME%\bin`

#### macOS/Linux
1. 编辑 Shell 配置文件（如 `~/.zshrc` 或 `~/.bashrc`）：
   ```bash
   # 设置 Oracle JDK 17 路径（示例路径）
   export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home"
   export PATH="$JAVA_HOME/bin:$PATH"
   ```
2. 生效配置：
   ```bash
   source ~/.zshrc  # 或 source ~/.bashrc
   ```

---

### **3. 常见问题与解决**
#### **问题 1：`java -version` 仍显示旧版本**
- **原因**：系统默认 Java 未指向 JDK 17。
- **解决**：
  - **Windows**：检查 `Path` 变量中 `%JAVA_HOME%\bin` 是否在旧版本路径之前。
  - **macOS/Linux**：
    ```bash
    # 查看所有 Java 版本路径
    /usr/libexec/java_home -V

    # 临时切换版本
    export JAVA_HOME=$(/usr/libexec/java_home -v 17)
    ```

#### **问题 2：IDE（如 IntelliJ/Eclipse）无法识别 JDK 17**
- **解决**：
  1. **IntelliJ**：  
     `File → Project Structure → SDKs → ➕ → JDK` → 选择 JDK 17 安装路径。
  2. **Eclipse**：  
     `Window → Preferences → Java → Installed JREs → Add → Standard VM` → 选择 JDK 17 路径。

#### **问题 3：权限不足（macOS/Linux）**
- **现象**：安装或执行时提示 `Permission denied`。
- **解决**：
  ```bash
  # 修改 JDK 目录权限
  sudo chmod -R 755 /Library/Java/JavaVirtualMachines/jdk-17.jdk
  ```

---

### **4. 多版本 JDK 管理**
#### Windows
- 在环境变量中直接修改 `JAVA_HOME` 和 `Path` 以切换版本。

#### macOS/Linux
- **使用 `jenv` 工具**：
  ```bash
  # 安装 jenv
  brew install jenv

  # 添加 JDK 17
  jenv add /Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home

  # 设置全局默认版本
  jenv global 17
  ```

---

### **5. 注意事项**
1. **Oracle JDK 许可证**  
   - 商用场景需遵守 [Oracle JDK 许可协议](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)。
2. **Android 开发兼容性**  
   - Android Studio 默认使用 OpenJDK，但 Oracle JDK 17 完全兼容。
3. **卸载旧版本**  
   - **Windows**：控制面板 → 卸载程序 → 删除旧版 Java。  
   - **macOS**：
     ```bash
     sudo rm -rf /Library/Java/JavaVirtualMachines/jdk-1.8.jdk
     ```

---

### **操作验证流程图**
```plaintext
安装 JDK 17 → 配置环境变量 → 验证版本 → 配置 IDE → 解决冲突（如有）
```

---

通过以上步骤，可确保 Oracle JDK 17 正确安装并集成到开发环境中。