# AutoDL + GitHub SSH Push 完整流程（适合小白）

本教程用于：

- 在 AutoDL 上开发代码
- 使用 Git 管理代码
- Push 到 GitHub private/public repo
- 避免 HTTPS 网络问题

推荐使用：

```text
GitHub SSH + Private Repo
```

因为：

- AutoDL 到 GitHub HTTPS 经常超时
- SSH 更稳定
- 更适合长期科研开发

---

# 1. 创建 GitHub Repository

GitHub：

```text
头像
→ Your repositories
→ New
```

创建：

```text
Repository name: VLA-Twin
Visibility: Private（推荐）
```

创建后不要关闭页面。

---

# 2. 进入 AutoDL 项目目录

```bash
cd /root/autodl-tmp/VLA-Twin
```

---

# 3. 初始化 Git（如果还没初始化）

```bash
git init
```

---

# 4. 配置 Git 用户信息（只需要一次）

```bash
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的GitHub邮箱"
```

例如：

```bash
git config --global user.name "har4yyyy"
git config --global user.email "hardyxie05@gmail.com"
```

检查：

```bash
git config --global --list
```

---

# 5. 添加代码并 Commit

添加所有文件：

```bash
git add .
```

Commit：

```bash
git commit -m "first commit"
```

---

# 6. 添加 GitHub Remote

把 GitHub repo 连接到本地项目：

```bash
git remote add origin https://github.com/你的用户名/仓库名.git
```

例如：

```bash
git remote add origin https://github.com/har4yyyy/VLA-Twin.git
```

---

# 7. 生成 SSH Key（最关键）

为什么需要？

```text
AutoDL → GitHub HTTPS
经常超时

SSH 更稳定
```

生成 SSH key：

```bash
ssh-keygen -t ed25519 -C "你的GitHub邮箱"
```

例如：

```bash
ssh-keygen -t ed25519 -C "hardyxie05@gmail.com"
```

一路按 Enter：

```text
Enter file in which to save the key:
Enter passphrase:
```

都直接按 Enter 即可。

成功后会显示：

```text
Your public key has been saved in ...
```

---

# 8. 查看 SSH 公钥

```bash
cat ~/.ssh/id_ed25519.pub
```

输出类似：

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI.... hardyxie05@gmail.com
```

复制整段。

---

# 9. 添加 SSH Key 到 GitHub

GitHub：

```text
头像
→ Settings
→ SSH and GPG keys
→ New SSH key
```

填写：

```text
Title: AutoDL
Key: 粘贴刚刚复制的整段内容
```

保存。

---

# 10. 测试 SSH 是否成功

```bash
ssh -T git@github.com
```

第一次会提示：

```text
Are you sure you want to continue connecting?
```

输入：

```text
yes
```

成功后：

```text
Hi xxx! You've successfully authenticated...
```

---

# 11. 将 Git Remote 从 HTTPS 改成 SSH

查看当前 remote：

```bash
git remote -v
```

如果显示：

```text
https://github.com/xxx/xxx.git
```

改成 SSH：

```bash
git remote set-url origin git@github.com:用户名/仓库名.git
```

例如：

```bash
git remote set-url origin git@github.com:har4yyyy/VLA-Twin.git
```

再次检查：

```bash
git remote -v
```

应该显示：

```text
origin  git@github.com:har4yyyy/VLA-Twin.git (fetch)
origin  git@github.com:har4yyyy/VLA-Twin.git (push)
```

---

# 12. Push 到 GitHub

```bash
git push -u origin main
```

如果 branch 是 master：

```bash
git push -u origin master
```

成功后：

GitHub 页面会出现：

- README
- 文件列表
- commit history

说明 push 成功。

---

# 13. 后续日常开发流程

修改代码后：

```bash
git add .
git commit -m "描述修改内容"
git push
```

即可。

---

# 推荐科研开发习惯

建议：

```text
每完成一个阶段就 commit 一次
```

例如：

```text
fix: setup Grounded-SAM2
feat: run step1 successfully
fix: transformers compatibility
```

因为：

- 大模型环境容易炸
- dependency 很复杂
- checkpoint 很多
- 随时可能配崩

及时 commit 非常重要。

---

# 常见错误

---

## 1. HTTP2 framing layer

```text
fatal: unable to access ...
HTTP2 framing layer
```

原因：

```text
AutoDL 到 GitHub HTTPS 不稳定
```

解决：

```text
改用 SSH
```

---

## 2. Permission denied (publickey)

```text
git@github.com: Permission denied (publickey)
```

原因：

```text
GitHub 没有添加当前机器 SSH key
```

解决：

```bash
cat ~/.ssh/id_ed25519.pub
```

复制后添加到 GitHub SSH Keys。

---

## 3. Failed to connect to github.com port 443

```text
Failed to connect to github.com port 443
```

原因：

```text
HTTPS 网络超时
```

解决：

```text
改 SSH
```

---

# 推荐配置（AutoDL）

推荐：

```text
Private Repo
+
SSH Push
+
数据盘存 checkpoint
+
频繁 commit
```

适合：

- AI
- Robotics
- VLA
- World Model
- Diffusion
- 大模型科研开发
