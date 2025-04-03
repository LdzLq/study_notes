要实现Jenkins连接GitLab项目并自动化执行代码更新和定时任务，按照以下步骤操作：

---

### **一、环境准备**
1. **安装Jenkins**  
   - 确保服务器已安装[Java JDK 11+](https://www.oracle.com/java/)
   - [下载Jenkins](https://www.jenkins.io/download/)并完成安装。

2. **安装必要插件**  
   - 进入 **Jenkins管理界面 > 插件管理 > 可用插件**，安装以下插件（plugin）：
     - **Git**（拉取Git代码）
     - **GitLab**（GitLab集成）
     - **Build Authorization Token Root**（用于Webhook触发）

---

### **二、配置GitLab连接**
1. **添加GitLab凭据**  
   - 进入 **Jenkins > 凭据 > 系统 > 全局凭据**，点击 **添加凭据**。
   - 选择类型 **SSH Username with private key** 或 **Username with password**（推荐SSH）。
   - 输入GitLab账号信息，如果是SSH，需粘贴私钥（默认在 `~/.ssh/id_rsa`）。

2. **配置GitLab服务器**（可选）  
   - 进入 **Jenkins > 系统设置 > GitLab**，填写GitLab URL和API Token（在GitLab的 **Settings > Access Tokens** 生成）。

---

### **三、创建Jenkins任务**
1. **新建任务**  
   - 点击 **新建Item**，选择 **Freestyle project** 或 **Pipeline**（推荐Pipeline）。

2. **配置Git仓库**  
   - 在 **源码管理** 中选择 **Git**。
   - 输入GitLab仓库URL（如 `git@gitlab.com:your/project.git`）。
   - 选择之前添加的凭据。
   - 设定分支（如 `main`）。


3. **设置触发器**  
   - **定时构建**（Cron表达式）：  
     在 **构建触发器 > 定时构建（Build periodically）** 输入表达式，例如 `H/15 * * * *` 表示每15分钟一次。
   - **GitLab Webhook触发（Build when a change is pushed to GitLab）**：  
     勾选 **Build when a change is pushed to GitLab**，记录生成的 **Webhook URL**（如 `http://<JENKINS_IP>/project/<任务名>`）。

4. **添加构建步骤**  
   - 如果是Shell脚本，在 **构建 > 执行Shell** 输入命令，例如：
     ```bash
     echo "开始构建"
     git pull origin main
     npm install
     npm run build
     ```
   - **输出结果乱码**：设置chcp 65001 解决乱码问题。

5. **添加构建后操作**
   - **Archive the artifacts**：
     在 **构建后操作 > 归档制品**，选择要存档的文件或目录。


---

### **四、配置GitLab Webhook**
1. **在GitLab中添加Webhook**  
   - 进入GitLab项目 **Settings > Webhooks**。
   - 填写Jenkins的Webhook URL，格式：  
     `http://<JENKINS_USER>:<JENKINS_TOKEN>@<JENKINS_IP>/job/<任务名>/build?token=<触发令牌>`  
     （用户和Token在Jenkins的 **用户设置 > API Token** 生成）。
   - 勾选 **Push events**，保存。

2. **测试Webhook**  
   - 点击 **Test > Push events**，检查Jenkins任务是否自动触发。

---

### **五、验证与排错**
1. **手动触发任务**  
   - 在Jenkins任务页点击 **立即构建**，查看控制台输出是否成功。

2. **检查日志**  
   - 如果Webhook失败，查看Jenkins的 **系统日志** 或GitLab的 **Webhook Logs**。

3. **常见问题**  
   - **权限问题**：确保Jenkins有GitLab仓库的拉取权限（SSH密钥或账号密码正确）。
   - **网络问题**：检查防火墙是否放行Jenkins端口（默认8080）。
   - **Cron语法错误**：使用[Cron表达式生成器](https://crontab.guru/)验证。

---

### **六、高级配置（可选）**
1. **Pipeline脚本**  
   在Jenkinsfile中定义自动化流程：
   ```groovy
   pipeline {
       agent any
       triggers {
           cron('H/15 * * * *')
           gitlab(triggerOnPush: true, branch: 'main')
       }
       stages {
           stage('Build') {
               steps {
                   sh 'git pull && npm install && npm run build'
               }
           }
       }
   }
   ```

2. **邮件通知**  
   - 安装 **Email Extension Plugin**，在任务配置中添加 **构建后操作 > 邮件通知**。

---
### **七、配置jenkins局域网访问**
1. 修改配置文件：`$JENKINS_HOME/jenkins.model.JenkinsLocationConfiguration.xml`
   * jenkinsURL修改为局域网IP地址，如：http://192.168.0.1:8080/
2. 重启Jenkins服务
3. 开启防火墙端口8080：
   * 打开“控制面板” → “Windows Defender 防火墙” → “高级设置”。
   * 新建入站规则，允许TCP端口8080。


