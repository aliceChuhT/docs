# BabelTTS 一次性授权与保护配置

日常维护者为 `aliceChuhT`（Alice）、`tabufan`。审核、合并、发布与回滚由两人负责，作者由另一人审核，
不要求 `BabelTTS` 逐次审核或合并。本 PR 仅修改仓库文件，不会自动变更以下平台设置。
首次检查时 Alice 仅有 READ 权限，因此本 PR 经个人 fork 提交；以下 Write 授权仍需确认生效。

| 顺序 | BabelTTS 一次性执行/协调 | 怎么验收 |
| --- | --- | --- |
| 1 | 在 `docs` 仓库授予/确认 `aliceChuhT`、`tabufan` 各自的 Write 权限，无需授予组织 Owner 或仓库 Admin | 两人均能创建仓库分支、审核他人 PR，并在门禁通过后合并；CODEOWNERS 识别两人为有效所有者 |
| 2 | 开启仓库 GitHub Actions，确认托管 Runner 和只读 token 策略；必要时批准首次 fork 工作流运行 | 非作者研发维护者完成首次工作流/脚本/基线审核。若首个 PR 没有触发 CI，维护者人工审核并首次合并后从 `master` 手动运行，提供 `docs-security` 成功链接；Checks 0 不算通过 |
| 3 | 为 `master` 启用 required PR、至少一次非作者审核、Code Owner review、最新变更重新审核、分支保持最新、required check `docs-security`；禁止强推/删除，不给 GitBook App 常规 bypass | Alice 提交的测试 PR 由 `tabufan` 审核，反向亦可；未审核、检查失败、审批后新增提交的 PR 均不能合并，审核及检查全部通过后日常维护者可以合并，无需领导审批 |
| 4 | 核查 GitBook App 授权：无需求则暂停/卸载，保留时仅选 `docs`；排查组织级 Secrets/Runner 对文档仓库的授权，取消业务生产权限 | 提供安装范围与访问配置证据；业务、合约、运维仓库不在范围，文档任务不能取得业务生产凭据或内网 Runner |
| 5 | 协调 GitBook 管理员分别给 Alice 和研发维护者对应实名账号的审核/发布权限；强制独立审核，限制普通编辑者直接发布；不把 BabelTTS 配为每次发布/部署的必需审批人 | 团队能完成编辑→非作者审核→发布→复核；未审核不可发布，普通编辑者不能管理权限。套餐不支持的控制明确列为缺口 |

`CODEOWNERS` 已配置 `* @aliceChuhT @tabufan`。两人必须都有真实 Write 权限，再启用 required Code Owner review；
该规则要求任一非作者维护者审核，而不是两人同时审批，更不是领导必审。不要把 `BabelTTS` 加回日常 CODEOWNERS 或部署必需审核人。

## 交由日常团队跟进

- Alice、研发和运维负责本次事件排查、凭据轮换、GitBook 整改、上线复核、告警与回滚演练。
- 调整 App 授权前保留原配置。2026-11-02 至 11-05 检查时刻的组织日志，若仅 Owner 可读，
  请一次性导出或配置合适的审计读取权限；团队负责分析，记录时区、覆盖范围与套餐造成的日志缺口。
- 独立静态站/CDN、受保护部署和监测由维护者与运维另提实施 PR，不作为领导日常审核任务。
- 若维护者调整或离职，另行更新授权、CODEOWNERS 与验证；不为了临时缺人把作者自审或 App bypass 打开。

配置完成请回传：执行人 / 验证时间 / 配置截图 / 成功 CI 链接 / 正反向审核验证结果 / 剩余缺口。
