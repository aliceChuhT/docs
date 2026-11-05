# BabelTTS 一次性授权与保护配置

日常维护者为 `aliceChuhT`（Alice）、`tabufan`。审核、合并、发布与回滚由两人负责，作者由另一人审核，
不要求 `BabelTTS` 逐次审核或合并。本 PR 仅修改仓库文件，不会自动变更以下平台设置。
首次检查时 Alice 仅有 READ 权限，因此本 PR 经个人 fork 提交；以下 Write 授权仍需确认生效。

以下设置页需使用 `BabelTTS` 的 GitHub Owner 身份打开；若返回 404 或没有设置入口，先确认登录账号。
按顺序执行：先授权、开启 Actions 和基础保护，再由团队完成首次合并/CI 验证，最后增加必需检查。
不要在工作流尚未跑通时添加必需检查，避免首次启用被卡住。

## 1. 授予两名维护者仓库 Write 权限（BabelTTS）

打开 [docs 仓库成员权限设置](https://github.com/decentralized-usd/docs/settings/access)，
导航为 **Settings → Collaborators & teams → Manage access**。

1. 找到 `aliceChuhT`、`tabufan`，分别将 Role 设为 **Write**。
2. 若列表没有对应账号，点击 **Add people**，搜索用户名，选择 **Write** 后添加。
3. 如出现待接受邀请，由本人先接受；不需要授予仓库 Admin 或组织 Owner。

验收：两人可在原仓库创建分支、审核他人 PR。两人均有真实 Write 权限后，CODEOWNERS 才能正确识别。
后续日常 PR 可直接从原仓库分支发起，不需要每次走个人 fork。

## 2. 开启并收紧 Actions（BabelTTS）

打开 [Actions → General](https://github.com/decentralized-usd/docs/settings/actions)，分别保存以下设置：

- **Actions permissions**：选择允许组织内部及选定外部 Actions 的选项，启用 **Allow actions created by GitHub**，以运行 PR 使用的 `actions/checkout`；无需允许所有第三方 Actions。
- **Workflow permissions**：选择 **Read repository contents and packages permissions**。
- 不开启 **Allow GitHub Actions to create and approve pull requests**。
- 保留外部 fork 工作流审批限制；如 [PR #1](https://github.com/decentralized-usd/docs/pull/1) 显示待批准，审阅后批准首次运行。
- 若设置被组织策略锁定，在 [组织 Actions 设置](https://github.com/organizations/decentralized-usd/settings/actions) 核实并仅为必要范围放行，不为此放宽全部仓库。

本次工作流使用 GitHub 托管 Runner、完整 SHA 固定的 Action 和只读 token，不需要 Secrets 或生产网络访问。

## 3. 建立 master 基础保护（BabelTTS）

打开 [Rulesets 设置](https://github.com/decentralized-usd/docs/settings/rules)，
点击 **New ruleset → New branch ruleset**，按下表配置后创建/保存：

| 设置 | 值 |
| --- | --- |
| Ruleset name | `docs-master-protection` |
| Enforcement status | **Active** |
| Target branches | **Include by pattern**，填写 `master` |
| Bypass list | 留空，不给 GitBook App 或日常维护者 bypass |
| Require a pull request before merging | 开启 |
| Required approvals | **1** |
| Dismiss stale pull request approvals when new commits are pushed | 开启 |
| Restrict deletions | 开启 |
| Block force pushes | 开启 |

此阶段先不添加 required status check，Code Owner 强制审核在第 5 步开启。
其余规则按实际需要另行评估；不要启用 **Restrict updates** 把所有日常合并都限制成只能由 bypass 身份操作。

## 4. 团队完成首次审核、合并与 CI 验证（Alice、tabufan）

1. `tabufan` 审核 Alice 提交的 [PR #1](https://github.com/decentralized-usd/docs/pull/1)，核实检查器、策略、基线和工作流；通过后由维护者完成首次合并。
2. 打开 [Actions 页面](https://github.com/decentralized-usd/docs/actions)，选择 **Documentation security**，确认任务 **docs-security** 绿色成功。
3. 若没有自动运行，在该工作流页面点击 **Run workflow**，Branch 选择 `master`，再点击运行。
4. 保存成功运行链接；**Checks 0 不算通过**。手动运行按钮要求工作流已进入默认分支，Write 权限即可操作，无需领导执行。

首次 PR 的目标分支尚无检查器或 CODEOWNERS，因此首次人工审核不能省略。
若 CI 在合并前已运行，应先确认成功；本步骤的人工首次启用流程不能用于绕过真实失败的检查。

## 5. 增加必需检查与 Code Owner 审核（BabelTTS）

首次 CI 成功后，回到 [Rulesets 设置](https://github.com/decentralized-usd/docs/settings/rules)，编辑上述规则：

- 开启 **Require review from Code Owners**。
- 开启 **Require status checks to pass**，添加真实运行过的 **docs-security**；若可选来源，选择 **GitHub Actions**。
- 开启 **Require branches to be up to date before merging**。
- 保存，确认规则仍为 **Active**、目标仍为 `master`、Bypass list 仍为空，其他基础规则保持启用。

[PR 中的 CODEOWNERS](https://github.com/aliceChuhT/docs/blob/codex/docs-security-release-controls/.github/CODEOWNERS) 已配置：

```text
* @aliceChuhT @tabufan
```

在 `master` 的 `.github/CODEOWNERS` 页面确认没有账号/权限错误。该规则要求任一非作者维护者审核；
审批数量设为 1，不要求两人同时审批，更不要求领导必审。不要将 `BabelTTS` 加回日常 CODEOWNERS 或部署必需审核人。

## 6. 收紧 GitBook App 和生产资源授权（BabelTTS，按需协调运维）

打开 [组织 GitHub Apps 设置](https://github.com/organizations/decentralized-usd/settings/installations)，找到 **GitBook → Configure**：

1. 修改前保存当前 Permissions、Repository access 和授权仓库列表。
2. 有同步需求：**Repository access → Only select repositories → 仅选 docs → Save**。
3. 无同步需求：选择 **Suspend** 暂停访问，不因恢复服务默认重新授权。
4. 确认业务、合约、运维仓库不在范围；同步方向不是只读权限的证明。

同时核查 [仓库 Actions Secrets](https://github.com/decentralized-usd/docs/settings/secrets/actions)、
[组织 Actions Secrets](https://github.com/organizations/decentralized-usd/settings/secrets/actions) 和
[仓库 Runner 设置](https://github.com/decentralized-usd/docs/settings/actions/runners)：
确保文档仓库不继承业务生产凭据、不使用能访问生产内网的共享 Runner。
优先移除不应授予 docs 的访问范围，不要误删其他业务仍在使用的组织级密钥。

## 7. 验证规则生效（Alice、tabufan）

使用测试 PR 留存以下结果，不在正式分支做破坏性测试：

- Alice 提交普通文档变更，未审核时不能合并。
- 另一个测试 PR 新增非白名单外链，预期 `docs-security` 失败并阻止合并；测试内容不合入正式分支。
- 审批后由原作者再提交变更，旧审批失效，需重新审核。
- 检查全部通过、`tabufan` 审核后，Alice 可合并，无需领导审批；反向由 tabufan 提交、Alice 审核也应成立。
- 确认普通账号和 GitBook App 均不在 Bypass list；关闭未合并的负向测试 PR，清理对应测试分支。

GitBook 审核/发布权限由其管理员另行一次性配置给两名日常维护者的实名账号，
验证团队可完成编辑→非作者审核→发布→复核，普通编辑者不能管理权限或未经审核直接发布。
套餐不支持的控制须记录缺口。GitHub Write 不会自动赋予 GitBook 发布权限，仓库检查也不能保护被供应商直接篡改的托管页面。

## 交由日常团队跟进

- Alice、研发和运维负责本次事件排查、凭据轮换、GitBook 整改、上线复核、告警与回滚演练。
- 调整 App 授权前保留原配置。2026-11-02 至 11-05 检查时刻的组织日志，若仅 Owner 可读，
  请一次性导出或配置合适的审计读取权限；团队负责分析，记录时区、覆盖范围与套餐造成的日志缺口。
- 独立静态站/CDN、受保护部署和监测由维护者与运维另提实施 PR，不作为领导日常审核任务。
- 若维护者调整或离职，另行更新授权、CODEOWNERS 与验证；不为了临时缺人把作者自审或 App bypass 打开。

配置完成请回传：执行人 / 验证时间 / 配置截图 / 成功 CI 链接 / 正反向审核验证结果 / 剩余缺口。

## 官方参考

- [成员权限管理](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings/managing-teams-and-people-with-access-to-your-repository)
- [Actions 配置](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository)
- [Rulesets 配置](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/creating-rulesets-for-a-repository)
- [Code Owners 权限与生效范围](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
- [手动运行工作流](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manually-run-a-workflow)
- [GitHub App 授权范围](https://docs.github.com/en/apps/using-github-apps/reviewing-and-modifying-installed-github-apps)
