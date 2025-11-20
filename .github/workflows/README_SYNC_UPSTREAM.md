# Upstream Sync Workflow

这个 GitHub Actions 工作流用于自动同步上游仓库 `iDvel/rime-ice` 的更新。

## 功能说明

- **定时运行**: 每周日 00:00 UTC 自动运行
- **手动触发**: 支持通过 GitHub Actions 界面手动触发
- **检查更新**: 自动检查上游仓库是否有新的提交
- **同步分支**: 将上游的 `main` 分支同步到本仓库的 `source` 分支
- **创建 PR**: 自动创建从 `source` 到 `main` 的 Pull Request
- **邮件通知**: 发送邮件通知有新的更新
- **手动合并**: 不会自动合并，需要手动审查和合并

## 配置步骤

### 1. 配置邮件服务器密钥

在仓库的 Settings > Secrets and variables > Actions 中添加以下密钥：

- `MAIL_SERVER`: 邮件服务器地址 (例如: smtp.gmail.com)
- `MAIL_PORT`: 邮件服务器端口 (例如: 587 或 465)
- `MAIL_USERNAME`: 邮件账户用户名
- `MAIL_PASSWORD`: 邮件账户密码或应用专用密码
- `MAIL_TO`: 接收通知的邮箱地址

### 2. 常见邮件服务器配置示例

#### Gmail
- Server: `smtp.gmail.com`
- Port: `587` (TLS) 或 `465` (SSL)
- 注意: 需要使用应用专用密码，不是普通密码

#### QQ 邮箱
- Server: `smtp.qq.com`
- Port: `587` 或 `465`
- 注意: 需要开启 SMTP 服务并使用授权码

#### 163 邮箱
- Server: `smtp.163.com`
- Port: `465`
- 注意: 需要开启 SMTP 服务并使用授权码

#### Outlook/Hotmail
- Server: `smtp-mail.outlook.com`
- Port: `587`

### 3. 分支设置

确保仓库有以下分支：
- `main`: 主分支
- `source`: 用于接收上游同步的分支（工作流会自动创建）

## 工作流程

1. 工作流每周日自动运行（或手动触发）
2. 检查上游仓库 `iDvel/rime-ice` 的 `main` 分支
3. 如果有更新：
   - 将上游更新同步到本仓库的 `source` 分支
   - 创建一个从 `source` 到 `main` 的 Pull Request
   - 发送邮件通知
4. 如果没有更新：
   - 记录日志，不执行任何操作

## 手动触发

1. 进入 GitHub 仓库页面
2. 点击 "Actions" 标签
3. 选择 "Sync Upstream and Create PR" 工作流
4. 点击 "Run workflow" 按钮
5. 选择分支并点击 "Run workflow"

## PR 合并流程

1. 收到邮件通知后，登录 GitHub
2. 查看新创建的 Pull Request
3. 仔细检查更改内容
4. 如果需要，在本地测试更改
5. 确认无误后，手动合并 PR

## 故障排查

### 工作流运行失败

1. 检查 Actions 日志查看具体错误信息
2. 确认邮件服务器密钥配置正确
3. 确认有足够的权限访问仓库

### 没有收到邮件

1. 检查邮件服务器密钥是否正确配置
2. 检查垃圾邮件文件夹
3. 检查邮件服务器是否允许 SMTP 发送

### PR 创建失败

1. 检查是否已有相同的 PR 存在
2. 检查 GitHub Token 权限
3. 查看 Actions 日志获取详细错误信息

## 注意事项

- 工作流使用 `GITHUB_TOKEN` 进行身份验证，无需额外配置
- `source` 分支会被强制更新以匹配上游，请不要在 `source` 分支上进行自定义修改
- 所有自定义修改应该在 `main` 分支或其他分支上进行
- 建议定期检查和合并 PR，避免积累过多更改

## 禁用工作流

如果需要暂时禁用自动同步：

1. 进入 `.github/workflows/sync-upstream.yml`
2. 注释掉 `schedule` 部分
3. 提交更改

或者在 GitHub Actions 界面直接禁用该工作流。
