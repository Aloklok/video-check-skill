# 视频标注审核 Skill（跨平台版）

适用于 WorkBuddy（Windows / macOS / Linux）。本仓库即 skill 本体，克隆到 WorkBuddy 的 skills 目录即可使用，无需因操作系统修改任何路径。

## 目录结构

- `SKILL.md`：skill 定义（审核流程、输出模板、两阶段流程等）
- `references/视频Caption审核规则总表.md`：单一规则总表（审核时加载）
- `references/参考文档/`：4 份源文档（溯源用，审核时不读）
- `README.md`：本文件
- `.gitignore`：排除媒体大文件

## 路径如何跨平台（重点）

本 skill 的参考文件随仓库一起打包在 `references/` 下，`SKILL.md` 中全部以**相对于本 SKILL.md 所在目录**的相对路径引用，例如：

- `references/视频Caption审核规则总表.md`
- `references/参考文档/`

WorkBuddy 在任意系统都会把 skill 安装到 `~/.workbuddy/skills/视频标注审核/`，因此相对路径在 Windows / macOS 下都能正确解析。**旧版硬编码了 `D:\0project\视频标注\...`，仅能在作者 Windows 上用；本版已改为相对路径，跨平台通用。**

> 如果你在自己的项目目录里也放了一份总表/参考文档，请忽略它们——本 skill 只读取自身 `references/` 里打包的那份，保证规则版本统一、跨机器一致。

## 安装（每台机器一次）

### macOS / Linux

```bash
git clone git@github.com:Aloklok/video-check-skill.git ~/.workbuddy/skills/视频标注审核
```

### Windows (PowerShell)

```powershell
git clone git@github.com:Aloklok/video-check-skill.git "$env:USERPROFILE\.workbuddy\skills\视频标注审核"
```

> 若该目录已存在旧的 `SKILL.md`，先把它移走/改名备份，再执行 clone（clone 要求目标目录为空或不存在）。

## 更新 / 同步（日常 push & pull）

skill 目录本身就是一个 git 仓库，所以「更新 skill」=「在仓库里 pull」：

- 在某台机器改了 `SKILL.md` 或 `references/` 后：

  ```bash
  git add -A && git commit -m "你的改动说明" && git push
  ```

- 在另一台机器拉取即可（无需手动复制文件）：

  ```bash
  cd ~/.workbuddy/skills/视频标注审核     # Windows 用 $env:USERPROFILE\.workbuddy\skills\视频标注审核
  git pull
  ```

pull 完 skill 立即生效，重启/刷新 WorkBuddy 会话即可。

### 在哪改（重要）

- **只改 skill 目录里这份**（`~/.workbuddy/skills/视频标注审核/`，Windows 为 `$env:USERPROFILE\.workbuddy\skills\视频标注审核\`）——它本身就是 git 仓库，改完按上面 push 即可。
- 构建/打包时可能在别处留过**临时副本**（如 Windows 上的 `D:\0project\视频标注\video-check-skill\`），那只用于打包，**不要在那儿改**；改了不会同步到 GitHub，还会造成版本混乱（可随时删除）。
- 旧版硬编码路径的 `SKILL.md.bak.*` 备份也不要动。

### 不想敲命令？直接说人话就行

日常更新无需手动敲 git：在对应机器上对 WorkBuddy 说「把对视频标注审核 skill 的修改提交并推送到 GitHub」，它会自动 `cd` 进该目录执行 `git add -A && git commit && git push`；另一台机器说「拉取视频标注审核 skill 的最新更新」即可 `git pull`。

> **不需要**为此再做一个"管理自己的 skill"，也**不需要**定时自动化——更新是事件驱动的（改完才推），git 原生操作最直接。本仓库就是 skill 本体兼同步源。

## 不进仓库的内容

- `*.mp4` / `*.wav` 等规则讲解视频、音频体积大，不进仓库（见 `.gitignore`）。本地资料自行保管。
- 待审样本、审核意见等属于各项目的工作文件，**不进本仓库**；它们应放在你本地的项目目录（如 Windows 的 `D:\0project\视频标注` 或 Mac 上的等价目录），与 skill 仓库分离。

## 飞书规则更新流程

1. 先改 `references/参考文档/` 中对应源文档；
2. 同步到 `references/视频Caption审核规则总表.md`，保持二者一致；
3. `git add -A && git commit && git push`；
4. 其他机器 `git pull` 即同步。
