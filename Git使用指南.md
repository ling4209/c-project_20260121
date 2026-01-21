# Windows11下C语言程序Git版本管理详细指南

## 1. 环境初始化与配置

### 1.1 Windows11下Git的安装步骤

1. **下载Git安装包**
   - 访问官方网站：https://git-scm.com/download/win
   - 下载最新版本的64位Git for Windows安装包

2. **安装Git**
   - 双击安装包，开始安装
   - 在"Select Components"页面，确保勾选：
     - Git Bash Here（右键菜单）
     - Git GUI Here（可选）
     - Git LFS（可选，大型文件支持）
   - 在"Choosing the default editor used by Git"页面，选择您熟悉的编辑器，推荐使用VS Code或Trae CN
   - 在"Adjusting the name of the initial branch in new repositories"页面，保持默认分支名为"main"
   - 在"Configuring the line ending conversions"页面，选择"Checkout Windows-style, commit Unix-style line endings"
   - 其他选项保持默认，点击"Install"完成安装

3. **验证Git安装**
   - 打开Git Bash（右键点击桌面，选择"Git Bash Here"）
   - 输入命令：
     ```bash
     git --version
     ```
   - 若显示Git版本信息，则安装成功

### 1.2 全局配置用户名/邮箱

- 打开Git Bash
- 执行以下命令配置全局用户名：
  ```bash
  git config --global user.name "Zhuoling"
  ```
- 执行以下命令配置全局邮箱：
  ```bash
  git config --global user.email "zhuolingmo@163.com"
  ```
- 验证配置：
  ```bash
  git config --global --list
  ```

### 1.3 Trae CN编辑器关联Git仓库

#### 初始化本地仓库
1. 打开Trae CN编辑器
2. 点击"文件" → "打开文件夹"，选择您的C项目目录（如`d:\mzl\project\git\20260120_2`）
3. 点击左侧导航栏的"源代码管理"图标（Git图标）
4. 点击"初始化仓库"按钮
5. 在弹出的对话框中选择您的项目目录，点击"初始化"

#### 关联远程仓库
1. 在GitHub上创建一个新的远程仓库（访问https://github.com/new）
2. 仓库名称建议与本地项目名一致
3. 选择"Public"或"Private"
4. 不要勾选"Initialize this repository with a README"等选项
5. 点击"Create repository"
6. 复制远程仓库的HTTPS地址（如`https://github.com/ling4209/my-c-project.git`）

在Trae CN中关联：
1. 点击"源代码管理" → "更多操作" → "远程"
2. 点击"添加远程仓库"
3. 输入名称（通常为"origin"）
4. 粘贴刚才复制的HTTPS地址
5. 点击"添加"

## 2. 基础Git命令实操

### 2.1 创建项目与编写首版代码

1. **创建项目结构**
   ```bash
   # 在Git Bash中执行
   mkdir my-c-project
   cd my-c-project
   mkdir src include
   ```

2. **编写首版代码**
   - 创建`src/main.c`文件：
     ```c
     #include <stdio.h>
     #include "../include/hello.h"
     
     int main() {
         hello_world();
         return 0;
     }
     ```
   - 创建`include/hello.h`文件：
     ```c
     #ifndef HELLO_H
     #define HELLO_H
     
     void hello_world();
     
     #endif
     ```
   - 创建`src/hello.c`文件：
     ```c
     #include <stdio.h>
     #include "../include/hello.h"
     
     void hello_world() {
         printf("Hello, World!\n");
     }
     ```

### 2.2 提交仓库的完整命令链

1. **初始化本地仓库**
   ```bash
   git init
   ```
   - 适用场景：首次创建项目时
   - 必填参数：无

2. **添加文件到暂存区**
   ```bash
   git add src/ include/
   ```
   - 适用场景：将修改的文件添加到暂存区，准备提交
   - 可选参数：
     - `.` ：添加所有修改的文件
     - 具体文件名：添加指定文件

3. **提交到本地仓库**
   ```bash
   git commit -m "Initial commit: Create basic C project structure with hello world program"
   ```
   - 适用场景：将暂存区的文件提交到本地仓库
   - 必填参数：
     - `-m "提交信息"`：提交的说明文字
   - 提交信息规范：
     - 首字母大写
     - 使用现在时态（如"Add"而不是"Added"）
     - 简洁明了，说明本次提交的主要内容
     - 示例：
       - "Fix: Correct division by zero in calculate_average function"
       - "Add: Implement matrix multiplication functionality"
       - "Update: Improve error handling in file operations"

4. **关联远程仓库**
   ```bash
   git remote add origin https://github.com/ling4209/my-c-project.git
   ```

5. **推送到远程仓库**
   ```bash
   git push -u origin main
   ```
   - 适用场景：首次将本地仓库推送到远程仓库
   - 必填参数：
     - `-u`：设置上游分支，后续推送可简化为`git push`

## 3. 长期迭代中的版本管理

### 3.1 修改C程序后的更新流程

#### 单个文件修改
```bash
# 修改文件后查看修改内容
git status

# 将单个文件添加到暂存区
git add src/hello.c

# 提交到本地仓库
git commit -m "Update: Change hello_world function to print customized message"

# 推送到远程仓库
git push
```

#### 多文件批量修改
```bash
# 查看所有修改内容
git status

# 将所有修改的文件添加到暂存区
git add .

# 提交到本地仓库
git commit -m "Refactor: Improve code structure in hello module"

# 推送到远程仓库
git push
```

### 3.2 查看代码修改记录

1. **查看所有提交历史**
   ```bash
   git log
   ```
   - 显示所有提交记录，按时间倒序排列

2. **查看简化的提交历史**
   ```bash
   git log --oneline
   ```
   - 每条记录显示为一行，包含提交哈希值和提交信息

3. **查看指定文件的历史版本**
   ```bash
   git log --oneline src/hello.c
   ```
   - 仅显示与`src/hello.c`相关的提交记录

4. **查看文件的具体修改内容**
   ```bash
   git show <commit哈希值> src/hello.c
   ```
   - 显示指定提交中对`src/hello.c`文件的具体修改

### 3.3 误改代码时的回滚方法

#### git reset的三种模式

| 参数 | 含义 | 适用场景 |
|------|------|----------|
| --soft | 仅撤销提交，保留暂存区和工作区的修改 | 提交信息写错，需要重新提交 |
| --mixed | 撤销提交和暂存区，保留工作区的修改 | 提交了不需要的文件，需要重新选择文件提交 |
| --hard | 完全撤销提交、暂存区和工作区的修改 | 代码修改错误，需要完全恢复到之前的版本 |

#### 具体示例

1. **回滚到上一次提交（保留修改）**
   ```bash
   git reset --mixed HEAD~1
   ```

2. **完全回滚到指定版本**
   ```bash
   # 查看提交历史，找到要回滚的版本哈希值
   git log --oneline
   
   # 完全回滚到指定版本
   git reset --hard a1b2c3d4
   ```

3. **撤销单个文件的修改**
   ```bash
   git checkout HEAD src/hello.c
   ```
   - 将`src/hello.c`恢复到当前HEAD指向的版本

### 3.4 将本地修改推送至远程仓库

完整流程：
```bash
# 1. 查看修改内容
git status

# 2. 添加修改的文件到暂存区
git add .  # 或指定具体文件

# 3. 提交到本地仓库
git commit -m "描述你的修改内容"

# 4. 推送到远程仓库
git push
```

首次推送新分支：
```bash
git push -u origin feature/new-function
```

## 4. 分支管理策略

### 4.1 适合单人迭代的分支模型

推荐使用**主分支+开发分支**的简化模型：
- **main分支**：稳定版本，仅包含已完成且测试通过的代码
- **dev分支**：开发分支，用于日常开发和功能实现
- **feature分支**：（可选）用于开发特定功能，完成后合并到dev分支

### 4.2 分支操作命令

1. **创建分支**
   ```bash
   git branch dev
   ```
   - 创建名为"dev"的新分支

2. **切换分支**
   ```bash
   git checkout dev
   ```
   - 切换到"dev"分支

3. **创建并切换分支**
   ```bash
   git checkout -b feature/new-function
   ```
   - 同时创建并切换到"feature/new-function"分支

4. **查看所有分支**
   ```bash
   git branch -a
   ```
   - 显示本地和远程的所有分支

5. **合并分支**
   ```bash
   # 切换到目标分支（如main）
   git checkout main
   
   # 合并dev分支到main分支
   git merge dev
   ```

### 4.3 Trae CN中可视化管理分支

1. **查看当前分支**
   - 在Trae CN的状态栏右下角，显示当前所在分支

2. **切换分支**
   - 点击状态栏的分支名称
   - 在弹出的分支列表中选择要切换的分支

3. **创建新分支**
   - 点击"源代码管理" → "更多操作" → "分支" → "创建分支"
   - 输入分支名称，点击"创建"

### 4.4 避免合并冲突的技巧

1. **保持分支同步**
   - 定期将main分支的更新合并到dev分支
   ```bash
   git checkout dev
   git merge main
   ```

2. **小步提交**
   - 每次提交只包含一个功能或修复，避免大量代码一次性提交

3. **及时解决冲突**
   - 合并时如果发生冲突，Trae CN会显示冲突文件
   - 点击文件中的冲突标记，选择"接受当前更改"、"接受传入更改"或"接受两者更改"
   - 解决所有冲突后，使用`git add`和`git commit`完成合并

## 5. C项目专属配置

### 5.1 .gitignore文件编写

在项目根目录创建`.gitignore`文件，添加以下内容：

```gitignore
# 编译产物
*.o
*.obj
*.out
*.exe
*.a
*.lib
*.dll
*.so

# 编译中间文件
*.i
*.s
*.map

# 临时文件
*.tmp
*.temp

# 编辑器配置文件
.vscode/
.idea/
*.swp
*.swo

# 日志文件
*.log

# 操作系统文件
.DS_Store
Thumbs.db

# 构建目录
build/
bin/
```

### 5.2 C项目Git配置建议

1. **使用标签管理版本**
   ```bash
   # 创建标签
git tag v1.0.0

# 推送标签到远程仓库
git push --tags
```

2. **忽略大文件**
   - 对于大型二进制文件，考虑使用Git LFS
   - 安装Git LFS：`git lfs install`
   - 跟踪大型文件：`git lfs track "*.exe"`

## 6. Trae CN与Git联动

### 6.1 直接完成提交操作

1. **查看修改内容**
   - 点击左侧导航栏的"源代码管理"图标
   - 在"更改"列表中查看所有修改的文件
   - 点击文件名可查看具体修改内容

2. **暂存文件**
   - 点击文件右侧的"+"图标，将文件添加到暂存区
   - 或点击"+"按钮暂存所有更改

3. **提交更改**
   - 在"消息"输入框中输入提交信息
   - 点击"提交"按钮完成提交

4. **推送到远程仓库**
   - 提交完成后，点击"同步更改"按钮
   - 选择"推送"将本地提交推送到远程仓库

### 6.2 拉取远程更新

- 点击"同步更改"按钮
- 选择"拉取"将远程仓库的更新拉取到本地

### 6.3 解决冲突

- 当拉取或合并时发生冲突，Trae CN会在"源代码管理"中显示冲突文件
- 点击冲突文件，在编辑区域查看冲突内容
- 使用编辑器提供的冲突解决工具，选择合适的更改
- 解决所有冲突后，暂存并提交更改

### 6.4 AI生成提交信息

- 在提交消息输入框中，点击"AI生成"按钮
- Trae CN会根据你的代码修改自动生成提交信息
- 可以编辑生成的信息，使其更准确
- 点击"提交"按钮完成提交

## 7. 常见问题排查

### 7.1 提交失败

**问题**：执行`git commit`时提示"Author identity unknown"
**解决**：
```bash
# 检查用户名和邮箱配置
git config --list

# 如果未配置，执行以下命令
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### 7.2 远程仓库同步失败

**问题**：执行`git push`时提示"fatal: The current branch main has no upstream branch"
**解决**：
```bash
git push -u origin main
```

**问题**：执行`git push`时提示"Permission denied"
**解决**：
- 检查远程仓库地址是否正确
- 确保你有权限访问该远程仓库
- 考虑使用SSH认证替代HTTPS认证

### 7.3 冲突提示

**问题**：合并分支时提示"CONFLICT (content): Merge conflict in file.c"
**解决**：
- 在Trae CN中打开冲突文件
- 解决所有冲突标记
- 暂存解决冲突后的文件
- 完成合并提交

### 7.4 C程序编译文件误提交后的移除方法

如果不小心提交了编译产物（如`.exe`、`.o`文件），可以使用以下命令移除：

```bash
# 从Git中移除文件，但保留本地文件
git rm --cached *.exe *.o

# 提交移除操作
git commit -m "Remove compiled files"

# 推送到远程仓库
git push
```

## 8. 长期C程序初版设计

### 8.1 项目结构

```
my-c-project/
├── include/          # 头文件目录
│   ├── calculator.h  # 计算器功能头文件
│   └── utils.h       # 工具函数头文件
├── src/              # 源代码目录
│   ├── main.c        # 主程序入口
│   ├── calculator.c  # 计算器功能实现
│   └── utils.c       # 工具函数实现
├── .gitignore        # Git忽略配置文件
└── README.md         # 项目说明文档
```

### 8.2 初版代码实现

1. **include/calculator.h**
```c
#ifndef CALCULATOR_H
#define CALCULATOR_H

// 基本算术运算函数
double add(double a, double b);
double subtract(double a, double b);
double multiply(double a, double b);
double divide(double a, double b);

#endif
```

2. **src/calculator.c**
```c
#include "../include/calculator.h"

// 加法函数
double add(double a, double b) {
    return a + b;
}

// 减法函数
double subtract(double a, double b) {
    return a - b;
}

// 乘法函数
double multiply(double a, double b) {
    return a * b;
}

// 除法函数
double divide(double a, double b) {
    // 简单的错误处理
    if (b == 0) {
        return 0; // 实际项目中应使用更完善的错误处理
    }
    return a / b;
}
```

3. **include/utils.h**
```c
#ifndef UTILS_H
#define UTILS_H

// 打印菜单
void print_menu();

// 清空输入缓冲区
void clear_input_buffer();

#endif
```

4. **src/utils.c**
```c
#include <stdio.h>
#include "../include/utils.h"

// 打印菜单
void print_menu() {
    printf("\n=== 简易计算器 ===\n");
    printf("1. 加法\n");
    printf("2. 减法\n");
    printf("3. 乘法\n");
    printf("4. 除法\n");
    printf("5. 退出\n");
    printf("请选择操作: ");
}

// 清空输入缓冲区
void clear_input_buffer() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
}
```

5. **src/main.c**
```c
#include <stdio.h>
#include "../include/calculator.h"
#include "../include/utils.h"

int main() {
    int choice;
    double a, b, result;

    while (1) {
        print_menu();
        
        // 读取用户选择
        if (scanf("%d", &choice) != 1) {
            printf("输入错误，请重新输入！\n");
            clear_input_buffer();
            continue;
        }
        
        // 退出程序
        if (choice == 5) {
            printf("感谢使用计算器！\n");
            break;
        }
        
        // 输入验证
        if (choice < 1 || choice > 5) {
            printf("选择错误，请输入1-5之间的数字！\n");
            clear_input_buffer();
            continue;
        }
        
        // 读取操作数
        printf("请输入第一个数字: ");
        if (scanf("%lf", &a) != 1) {
            printf("输入错误，请重新输入！\n");
            clear_input_buffer();
            continue;
        }
        
        printf("请输入第二个数字: ");
        if (scanf("%lf", &b) != 1) {
            printf("输入错误，请重新输入！\n");
            clear_input_buffer();
            continue;
        }
        
        // 执行运算
        switch (choice) {
            case 1:
                result = add(a, b);
                printf("%.2f + %.2f = %.2f\n", a, b, result);
                break;
            case 2:
                result = subtract(a, b);
                printf("%.2f - %.2f = %.2f\n", a, b, result);
                break;
            case 3:
                result = multiply(a, b);
                printf("%.2f * %.2f = %.2f\n", a, b, result);
                break;
            case 4:
                if (b == 0) {
                    printf("错误：除数不能为0！\n");
                } else {
                    result = divide(a, b);
                    printf("%.2f / %.2f = %.2f\n", a, b, result);
                }
                break;
        }
        
        clear_input_buffer();
    }
    
    return 0;
}
```

6. **README.md**
```markdown
# 简易计算器C程序

一个使用C语言开发的简易命令行计算器，支持基本的算术运算。

## 功能特点

- 支持加法、减法、乘法、除法运算
- 简单的错误处理
- 清晰的菜单界面
- 模块化设计，便于扩展

## 如何编译

使用GCC编译器：

```bash
gcc src/main.c src/calculator.c src/utils.c -I include -o calculator.exe
```

## 如何运行

```bash
./calculator.exe
```

## 项目结构

- `include/`：头文件目录
- `src/`：源代码目录
- `.gitignore`：Git忽略文件配置
- `README.md`：项目说明文档

## 版本管理

本项目使用Git进行版本管理，详细的Git使用指南请参考本仓库的Git指南文档。
```

## 9. 总结

本指南详细介绍了Windows11下C语言程序使用Git进行版本管理的完整流程，包括：
- 环境初始化与配置
- 基础Git命令实操
- 长期迭代中的版本管理
- 分支管理策略
- C项目专属配置
- Trae CN与Git联动
- 常见问题排查
- 长期C程序初版设计

希望这份指南能帮助你在Windows11环境下，使用Trae CN编辑器和Git高效地管理C语言程序的版本。在实际开发中，建议：

1. 坚持小步提交，每次提交只包含一个功能或修复
2. 保持提交信息清晰、规范
3. 定期备份代码到远程仓库
4. 根据项目规模灵活调整分支策略
5. 不断学习和实践Git的高级功能

祝你编程愉快！