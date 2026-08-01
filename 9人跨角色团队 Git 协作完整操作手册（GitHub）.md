# 9人跨角色团队 Git 协作完整操作手册（GitHub）

# 9 人跨角色团队 Git 协作完整操作手册（GitHub）

团队构成：项目经理、产品经理、开发、测试
仓库地址：`https://github.com/fireflygoup/mylangchain.git`

## 核心架构（适配 9 人中小团队，平衡管控与效率）

### 长期常驻分支（永久保留）

1. **main【生产分支｜保护分支】**
线上运行代码，**任何人禁止直接 push**，只能 PR 合并；合并代表版本可上线。

2. **develop【测试分支】**
所有开发功能集成分支；测试人员在此分支开展完整测试。

> 流程链路：feature 分支 → develop → main
> 
> 

### 临时短期分支（功能上线 / 修复完成后立即删除）

1. `feature/需求编号-功能简述` 【开发新需求】
示例：`feature/T001-报表导出`

2. `bugfix/问题编号-缺陷简述` 【测试 / 线上 bug 修复】
示例：`bugfix/B005-导入崩溃`

3. `hotfix/问题简述` 【线上紧急故障修复】

---

# 一、角色分工规范

1. **项目经理 PM**
负责管理需求编号、把控上线节点，审核 PR 合并权限。

2. **产品经理**
确认需求范围，协助区分普通 bug 和线上紧急故障 hotfix。

3. **后端 / 前端开发**
新建 feature 分支开发；自测完成后推送分支，提 PR 合并至 develop。

4. **测试人员**
基于 develop 分支打包测试；缺陷反馈开发，验证通过后通知项目经理。

# 二、成员首次接入项目（所有人仅执行一次）

打开 PowerShell，逐条运行

```powershell
# 1.克隆项目到本地
git clone https://github.com/fireflygoup/mylangchain.git
cd mylangchain

# 2.统一本地默认分支配置
git config --global init.defaultBranch main

# 3.配置用户名邮箱（和github账号保持一致）
git config --global user.name "你的姓名"
git config --global user.email "github注册邮箱"
```

# 三、完整标准工作流（每日循环操作步骤）

## 阶段 1：开发人员开始新需求

1. **同步最新代码（必做，防止大量冲突）**

```powershell
#切换到develop分支
git checkout develop
#拉取远程最新代码
git pull
```

2. **从 develop 拉出独立功能分支（一条需求 = 一条分支，禁止多人共用）**

```powershell
git checkout -b feature/T001-报表导出
```

3. 本地编写代码

4. 开发自测完成，提交代码

```powershell
git add .
git commit -m "【T001】实现报表导出功能，支持Excel"
```

> ✅提交注释格式：【需求编号】清晰描述改动
> 
> 

5. 将功能分支推送远程仓库

```powershell
git push https://github.com/fireflygoup/mylangchain.git feature/T001-报表导出
```

6. **网页端创建 PR（Pull Request）**

- 源分支：`feature/T001-报表导出`

- **目标分支：develop（重点！不要选 main）**

- 评审人：指定 1 名开发 \+ 测试
填写内容：需求编号、改动范围、自测要点

## 阶段 2：代码评审 \& 测试验证

1. 开发同事代码评审，确认代码质量

2. PR 审核通过后，合并到`develop`分支

3. 测试人员拉取最新 develop 分支打包，开展测试

```powershell
git checkout develop
git pull
```

## 阶段 3：测试无 bug，准备上线

1. 项目经理确认本轮所有需求测试全部通过

2. 项目经理发起 PR：`develop` → `main`

3. 上线完成

## 阶段 4：上线完成清理工作

开发人员本地清理废弃功能分支

```powershell
git checkout develop
git pull
#删除本地旧分支
git branch -d feature/T001-报表导出
#可选：项目经理删除远程废弃feature分支
git push https://github.com/fireflygoup/mylangchain.git --delete feature/T001-报表导出
```

# 四、线上紧急故障 Hotfix 单独流程

⚠️场景：线上 main 出现严重 bug，不能等待下一轮迭代

1. 开发从 **main 分支**拉出修复分支

```powershell
git checkout main
git pull
git checkout -b hotfix/支付报错修复
```

2. 修改代码提交

3. 推送分支，创建 2 条 PR

    - PR1：`hotfix/xxx` → `main`（修复线上）

    - PR2：`hotfix/xxx` → `develop`（同步修复到测试环境，避免下次迭代复现）

# 五、高频问题：代码冲突标准处理步骤

拉取代码出现冲突时

1. 使用 VSCode 打开冲突文件，选择保留代码

2. 全部冲突处理完毕

```powershell
git add .
git commit -m "合并：解决文件冲突"
```

> 多人协作不要强行覆盖别人代码，无法解决冲突立刻拉相关开发沟通
> 
> 

# 六、所有人必须遵守【红线禁令】

❌ 禁止直接在 main、develop 分支本地写代码
❌ 禁止直接执行 `git push` 推送 main/develop
❌ **全程禁止使用 git push \-\-force 强制推送！会覆盖团队其他人代码**
❌ 一条需求不要拆分多个 feature 分支，不要多个需求塞在同一个分支
✅ 任何改动必须新建临时分支
✅ 每次开发前必须先 `git pull` 同步远端 develop
✅ 分支名称严格统一命名格式，便于所有人识别

# 七、GitHub 仓库管理员网页配置（项目经理一次性设置）

仓库主页 → Settings → Branches → Add rule

## 规则 1：保护 main 分支

Branch name pattern：`main`
勾选：

1. Require pull request reviews before merging 合并需要审核

2. Do not allow bypassing the above settings 禁止绕过规则强制推送

## 规则 2：保护 develop 分支

Branch name pattern：`develop`
勾选：

1. Require pull request reviews before merging

> 设置完成后，任何人直接推送 main、develop 会报错，强制走 PR 流程
> 
> 

# 八、常用命令速查表（可以截图保存）

```powershell
#查看所有本地分支
git branch
#查看本地+远程所有分支
git branch -a

#切换分支
git checkout 分支名称

#创建并切换新分支
git checkout -b feature/xxx

#删除本地分支
git branch -d feature/xxx
#删除远程废弃分支
git push https://github.com/fireflygoup/mylangchain.git --delete feature/xxx
```

如果你需要，我再输出一份**纯无格式文本版本**，你直接复制发送团队微信群 / 钉钉。

