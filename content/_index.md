---

title: "SRE Knowledge Vault"
type: "docs"

---


### 构建高可用架构，沉淀自动化运维最佳实践。

- 个人技术Vault
- 排错日常更新！！

---

### 快速导航

请直接选择你需要查阅的模块：

{{< cards >}}
  {{< card link="/incident-runbook/" title="🚑 应急排障手册 (Runbook)" subtitle="生产环境故障处理 SOP 与踩坑记录" >}}
  {{< card link="/blog/" title="📝 技术博客模块" subtitle="关于架构设计、云原生与效能优化的深度思考" >}}
  {{< card link="/blog/Devops_Skill/" title="⚙️ DevOps 实践" subtitle="CI/CD 流水线、IaC 与自动化运维脚本" >}}
  {{< card link="/blog/about/" title="👨‍💻 关于作者" subtitle="了解我的履历、技术栈与联系方式" >}}
{{< /cards >}}

---

### 架构原则

始终坚守以下 SRE 核心原则：

*   **IaC (基础设施即代码)：** 拒绝手动操作，凡是能用 Terraform 或 Ansible 描述的，绝不妥协于控制台点击。

*   **Observability (可观测性)：** 监控先行。没有指标度量的系统，就像蒙眼狂奔的汽车。

*   **Design for Failure (面向失败设计)：** 永远假设任何云组件都会在下一秒宕机，通过冗余和容灾保证系统韧性。

> "SRE is what happens when you ask a software engineer to design an operations team." 
> — *Benjamin Treynor Sloss, Google*