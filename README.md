# Ansible 自动化批量部署运维平台（实训项目）

## 项目简介
基于 Ansible 的 **SSH 无 Agent** 架构，对多台 Ubuntu 服务器进行统一批量管理，替代逐台手动登录操作，实现 Nginx、Prometheus 的批量安装部署、配置下发、服务启停与开机自启。项目采用 Playbook（YAML）编写自动化剧本，并使用 Roles 工程化拆分任务、Handlers 触发器在配置变更时自动重载服务，脚本具备**幂等性**（重复执行不会重复安装软件）。

## 实验环境
| 角色 | 系统 | IP（示例） | 说明 |
| ---- | ---- | ---------- | ---- |
| 控制节点 | Ubuntu 22.04 | 192.168.1.10 | 安装 Ansible，管理其他节点 |
| 被管节点 web01 | Ubuntu 22.04 | 192.168.1.11 | 部署 Nginx |
| 被管节点 web02 | Ubuntu 22.04 | 192.168.1.12 | 部署 Prometheus |

> 所有节点用户名：ubuntu；节点间网络互通，控制节点与被管节点完成 SSH 密钥免密认证。

## 技术栈
Ansible、YAML、SSH 免密登录、Inventory 主机清单、Playbook、Roles、Handlers、Nginx、Prometheus

## 目录结构
```
ansible-ops-lab/
├── README.md                  # 项目说明（本文件）
├── .gitignore                 # 忽略敏感文件与缓存
├── docs/
│   └── 实训步骤.md             # 完整实训操作步骤
├── playbooks/
│   ├── deploy_nginx.yml        # 批量部署 Nginx 剧本
│   ├── deploy_prometheus.yml   # 批量部署 Prometheus 剧本
│   └── role_deploy.yml          # 调用 web_role 的入口剧本
├── roles/
│   └── web_role/
│       ├── tasks/main.yml      # 角色任务
│       ├── handlers/main.yml   # 触发器：配置变更后重载 Nginx
│       └── templates/
│           └── nginx.conf.j2   # Jinja2 配置模板
└── screenshots/                # 实训实验截图（自行放入）
```

## 实训目标
1. 理解 Ansible 无 Agent 架构，掌握 Inventory 主机清单分组管理。
2. 编写 Ad-hoc 临时命令完成批量巡检与文件分发。
3. 编写 Playbook 实现 Nginx、Prometheus 批量部署，理解**幂等性**。
4. 使用 Roles 工程化拆分任务，使用 Handlers 实现配置变更自动重载。

## 快速开始
```bash
# 1. 配置 SSH 免密（见 docs/实训步骤.md）
# 2. 测试连通性
ansible webservers -m ping

# 3. 批量部署 Nginx
ansible-playbook playbooks/deploy_nginx.yml

# 4. 使用 Role 部署 Web 服务
ansible-playbook playbooks/role_deploy.yml
```
