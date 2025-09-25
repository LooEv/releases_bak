# 📦 GitHub 仓库备份工具

[English README](README.en.md)

这是一个基于 **GitHub Actions** 的自动化仓库备份工具。
它可以每天定时或手动触发，自动拉取你指定的 GitHub 仓库的 **最新 Release** 或 **指定分支源码** 并保存到本仓库中，方便长期归档。
👉 [查看备份仓库列表](backup-repos.md)

## ✨ 功能

* 自动检查并下载目标仓库的最新 **Release**（含 tarball、zipball、assets）。
* 如果没有 Release，则自动备份指定分支（默认是仓库的默认分支）。
* 自动生成一份 `backup-repos.md` 文档，列出所有正在备份的仓库的关键信息（描述、Star、Fork、最近更新）。
* 支持 **每天定时执行**（北京时间 8:00，可修改）或 **手动触发**。

---

## 📂 文件说明

* `backup-repos.url`
  存放要备份的 GitHub 仓库链接，每行一个。

  * 支持直接写仓库地址：

    ```
    https://github.com/OWNER/REPO
    ```
  * 也可以指定分支（`tree/branch-name`）：

    ```
    https://github.com/OWNER/REPO/tree/branch-name
    ```
  * 行首 `#` 表示注释，会被忽略。
  * 空行会被忽略。

* `backup-repos.md`
  由 GitHub Action 自动生成，包含当前所有备份仓库的信息表格。
  **无需手动修改**。

* `.github/workflows/backup.yml`
  工作流配置文件（本文档顶部的 YAML 内容）。
  控制如何执行自动备份任务。

* `releases/`
  自动生成的目录，保存所有下载下来的 Release 包或分支源码。

---

## 🚀 使用方法

1. **创建一个新的仓库**（建议设为私有，避免占用公共资源）。
2. 在新仓库中创建 `.github/workflows/backup.yml` 文件，内容就是本项目提供的 GitHub Action 配置。
3. 在仓库根目录添加一个 `backup-repos.url` 文件，填入你要备份的目标仓库地址，例如：

   ```txt
   # 示例：直接写仓库地址
   https://github.com/torvalds/linux

   # 示例：指定分支
   https://github.com/vitejs/vite/tree/main

   # 注释行（不会执行）
   # https://github.com/xxxx/xxxx
   ```
4. 提交并推送代码到 GitHub。
5. 打开仓库的 **Actions** 页面，可以看到 `Backup Releases` 工作流：

   * 它会在 **每天北京时间 8:00（可修改） 自动执行**。
   * 也可以在页面上点击 **Run workflow** 手动执行。

执行完成后，你会在仓库中看到：

* 下载好的 Release 或源码包（存放在 `releases/OWNER/REPO/...`）。
* 自动生成或更新的 `backup-repos.md` 文件。

---

## 📖 注意事项

* 仓库可能很大，备份时请注意 GitHub 仓库大小限制（建议只备份重要项目）。
* 如果目标仓库被删除或设为私有，脚本会跳过并在日志中提示。
* `backup-repos.md` 只有在 `backup-repos.url` 修改后才会重新生成。
* **需要在仓库 Settings → Actions → General → Workflow permissions 中开启 `Read and write permissions`，否则无法推送备份结果。**

生成的 [backup-repos.md](backup-repos.md) 大概是这样的：

| 仓库                                                  | 描述                                   | Star   | Fork    | 最近更新       |
| --------------------------------------------------- | ------------------------------------ | ------ | ------- | ---------- |
| [torvalds/linux](https://github.com/torvalds/linux) | Linux kernel source tree             | ⭐ 167k | 🍴 50k  | 2025-09-23 |
| [vitejs/vite](https://github.com/vitejs/vite)       | Next generation frontend tooling     | ⭐ 65k  | 🍴 5.2k | 2025-09-20 |
| [vuejs/core](https://github.com/vuejs/core)         | Vue.js is a progressive framework... | ⭐ 42k  | 🍴 8.1k | 2025-09-22 |
