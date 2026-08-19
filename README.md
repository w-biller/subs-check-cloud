# subs-check 免费云端定时运行（GitHub Actions）

让 GitHub 免费服务器每天定时运行 subs-check 检测，结果自动推回仓库，订阅链接永久在线。
**无需自己的电脑开机、无需 Docker、无需服务器。**

## 一、部署步骤（约 5 分钟）

1. 注册/登录 GitHub：https://github.com
2. 新建仓库：右上角 **+ → New repository**
   - Repository name：例如 `subs-check`
   - **Visibility 选 Public（公开）**（公开仓库 Actions 免费额度无上限；私有仓库每月仅 2000 分钟，建议把定时改成 2 次/天）
   - 不要勾选 Add README（保持空仓库）
3. 进入仓库后点 **uploading an existing file**（上传文件），把本文件夹里的内容拖进去：
   - `.github\workflows\subs-check.yml`
   - `config\config.yaml`
   - `.gitignore`
   - 本说明 `README.md`（可选）
   - 直接网页拖拽即可，会自动识别目录结构
4. 提交后打开 **Actions** 标签页，会看到 `subs-check 定时检测` 工作流自动开始跑（首次也可点 **Run workflow** 手动触发）
5. 等第一次运行结束（约 10~20 分钟），**Actions → 最新一次运行**，展开“提交检测结果”步骤可看到是否成功

## 二、订阅地址（给你的 Clash / v2ray 客户端用）

把 `你的用户名` 和 `你的仓库名` 替换进去：

- Clash / Mihomo 全量节点：`https://raw.githubusercontent.com/你的用户名/你的仓库名/main/output/all.yaml`
- Mihomo 带规则：`https://raw.githubusercontent.com/你的用户名/你的仓库名/main/output/mihomo.yaml`
- Base64 格式：`https://raw.githubusercontent.com/你的用户名/你的仓库名/main/output/base64.txt`

> 国内访问 raw 慢时，套一层加速（推荐 ghfast.top）：
> `https://ghfast.top/https://raw.githubusercontent.com/你的用户名/你的仓库名/main/output/all.yaml`

也可以直接在仓库网页里打开 `output/all.yaml` → Raw 复制链接，格式一样。

## 三、后续维护

- **改订阅源**：在仓库里编辑 `config/config.yaml` 的 `sub-urls`，提交后下一次自动生效
- **手动跑一次**：Actions 页面 → 左边选中工作流 → **Run workflow**
- **改频率**：编辑 `subs-check.yml` 里的 `cron`（UTC 时间）。默认每天 4 次；私有仓库建议改成 `'0 4,16 * * *'`（每天 2 次）
- **历史节点**：`keep-days: 7` 会让 `output/history/` 保留 7 天快照，下次把上次可用节点一起复测，已随输出自动提交

## 四、可选：GitHub Pages 分发（更快的 HTTPS 链接）

1. 仓库 Settings → Pages → Source 选 `Deploy from a branch` → 分支 `main` → 目录 `/ (root)` → Save
2. 之后订阅地址变成：`https://你的用户名.github.io/你的仓库名/output/all.yaml`
3. 再套 ghfast.top：`https://ghfast.top/https://你的用户名.github.io/你的仓库名/output/all.yaml`

## 五、说明与限制

- 每次运行会重新检测一轮（拉取订阅 → 测活 → 流媒体 → 测速），完成后自动提交 `output/` 目录；`node.exe`、`sub-store.bundle.js` 等大文件已被 `.gitignore` 排除，不会占仓库空间
- 云服务器在境外，无需 github-proxy，配置里已关闭；本地那份 `config\config.yaml` 继续保留 ghfast.top 加速，两者互不影响
- 工作流每天最多 4 次，公开仓库不消耗任何费用；超出 GitHub 限额时调度会自动跳过，不影响仓库
