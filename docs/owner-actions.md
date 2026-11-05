# BabelTTS 管理员待办

以下都是待验证的平台动作；合并本 PR 不会替管理员执行。组织成员列表当前显示 `BabelTTS` 为 Owner，
`aliceChuhT` 对本仓库的实际权限为 READ，需经个人 fork 提交 PR。

| 优先级 | 执行与验收 | 所需身份 |
| --- | --- | --- |
| P0 | 修改授权前保存 GitBook App 权限、完整授权仓库及 2026-11-02 至 11-05 检查时刻的原始审计日志；记录时区，覆盖北京时间边界。Git/API 日志若套餐不支持须标明缺口，不把无数据当无异常 | GitHub Owner；按需联系 GitBook 管理员 |
| P0 | 核查安装/卸载/扩权、仓库授权、异常推送及强推、保护规则、Actions、成员/Deploy Key/Secrets 变化。关联事件报告 IOC，记录异常处置与未排除风险 | GitHub Owner |
| P0 | 无需求则暂停/卸载 GitBook App；保留时只能选择文档仓库，不允许 All repositories。确认业务/合约/运维仓库均不在范围，验证正常文档操作。同步方向不能代表 App 只读 | GitHub Owner |
| P0 | 排查文档仓库及组织继承的 Secrets、Deploy Keys、共享 Runner；移除业务凭据与生产网络访问，暴露过的凭据在发行端轮换并验证旧值失效 | Owner/仓库 Admin/凭据负责人 |
| P0 | GitBook 使用实名账号与可用 MFA/SSO；分离编辑、审核、发布与管理员。清理不用的 Token/MCP/集成，禁止存入内部敏感资料；用普通账号验证不能管理权限或直接发布 | GitBook 管理员（不是 GitHub 登录权限） |
| P1 | 人工审核并合并首次 bootstrap PR；若 fork 工作流待批准，审阅后批准运行。在 Actions 中确认 `docs-security` 实际运行成功 | 仓库维护者/Admin |
| P1 | 在 `master` 启用 required PR、至少一次非作者审核、Code Owner review、最新变更重新审核、required check `docs-security`；阻止强推/删除，不给 GitBook App 常规 bypass | GitHub Owner/仓库 Admin |
| P1 | 确认 CODEOWNERS 中 `BabelTTS` 有效；若改为团队，团队必须真实存在且具有适当仓库权限。测试未审核、检查失败 PR 无法合并，并验证普通账号/App 无法绕过 | GitHub Owner/仓库 Admin |
| P1 | 确认 Git Sync 不能直接推正式分支；GitBook 有强制审核能力时配置并验证未审核不可发布。套餐缺失时记录缺口，不能宣称仓库规则已保护 GitBook 网页 | 两端管理员 |
| P2 | 按发布规范建设独立静态站与受保护部署环境，移除 GitBook 正式交付权限；建设独立监测、可信备份和回滚，并实测告警与恢复 | Owner、GitBook 管理员、运维 |

每项回传：状态 / 执行人 / 验证时间 / 配置截图或测试日志 / 剩余风险。
新授权、域名切换及凭据变更按当次审批执行，不在审计时顺便扩大权限。
