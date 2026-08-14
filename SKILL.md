---
name: 归一同步
description: "部署词一键同步 — SSoT源→四端部署+全量审计。触发: 归一/同步/部署同步/版本同步/全量检查"
version: "1.5"
domain: META
tags: [deployment, sync, 归一, audit, version-control]
last_updated: 2026-07-26
changelog:
  v1.5: 2026-07-26, INDEX.md双向比对增强(原索引活化折叠并入)
  v1.4: 2026-07-25, frontmatter补全+last_updated注入
---

# 归一同步 v1.3

> 万法归一。技能库域前缀时代 — 五端一致性验证 + 全量审计
> v1.3: 技能库域前缀架构适配 / 五端同步 / 全量引用审计 / KG结晶联动

## 核心架构 (2026-06-28 现状)

```
SSoT (.)          ← 域前缀命名(10域)
    │  域: DEV / WRITE / MEDIA / ART / WORK / STUDY / THINK
    │      MEMORY / FINANCE / META / SECURITY / ORCHESTRATE
    │
    ├──→ 部署词/ (启动词系统.html v9.4 + 核心指令)
    ├──→ Trae (配置: $HOME/.trae-cn/memory/ 或 $TRAE_HOME)
    ├──→ Hermes (配置: $HERMES_HOME 或 APPDATA标准路径)
    └──→ .trae本地 (<.trae-skills>\ 或 $LOCAL_TRAE_SKILLS)

注意: 用户路径(C:\Users\<USER>\记忆中枢\技能库\` 存在且可读
2. 技能库域前缀覆盖率扫描:
   - 扫描所有一级子目录, 统计是否有未加域前缀的目录
   - 若存在 `humanizer` / `creative` / `apple` 等无前缀目录 → 标记待处理
3. 读取各目标端版本标识:
   - 启动词系统.html → `v\d+\.\d+`
   - 技能库 INDEX.md → 最新同步时间戳
   - Trae家里/公司/Hermes → trae-喵老大核心词.txt head 1 版本号
4. 输出: 版本差异表 + 域前缀覆盖率报告, 确认后进 Phase 1

### Phase 1: 五端同步

| 端 | 目标路径 | 特殊处理 |
|----|---------|----------|
| **部署词** | <部署词>\ | 核心指令 + 启动词系统.html + trae-喵老大核心词.txt |
| **Trae家里** | C:\Users\<USER>\.trae-cn\memory\ | Copy-Item 全量部署词 |
| **Trae公司** | C:\Users\<USER>\.trae-cn\memory\ | + hooks.json WS→PS语法转换; 路径ROG→SZR751 |
| **Hermes** | %APPDATA%\cn.org.hermesagent.desktop\runtime\hermes-home\ | 源SOUL.md→default+max-dodo profile |
| **.trae本地** | <.trae-skills>\ | 技能库全量镜像同步(仅SKILL.md) |

每端完成后自验证: `Test-Path 目标文件` + 版本号/哈希对比

### Phase 2: 全量引用审计 (v1.3新增)
1. **技能库路径引用检查**:
   - 全量扫描所有 SKILL.md 中对旧路径的引用
   - 检查模式: `<技能库>\`, `00-技能源码/`, `DEV-`, `WRITE-` 等旧前缀
   - 修复: `content.replace()` 精确匹配, 禁止用patch工具修路径
2. **域路由一致性检查**:
   - 检查 `META-域路由\SKILL.md` 中的域映射表是否匹配当前技能库目录
   - 检查 `启动词系统.html` 中的指令词→域映射是否匹配
3. **INDEX.md 完整性检查 (含双向比对)**:
   - 正向: INDEX.md 中每条条目 → 磁盘对应 SKILL.md 是否存在
   - 反向: 磁盘所有 SKILL.md → INDEX.md 是否有对应条目
   - 域前缀: 确保所有域前缀目录在 INDEX.md 中有对应条目
   - 命名: 检查 name 字段与目录名的对应关系
   - 差异输出: 索引多余条目清单 / 磁盘未注册技能清单
4. **断链检测**:
   - `extends:` 字段 → 检查目标 SKILL.md 是否存在
   - 路径引用 → 检查磁盘上目录是否存在

### Phase 3: 知识库联动同步 (v1.3新增)
| 知识库 | 同步内容 | 频率 |
|--------|----------|------|
| **KG结晶** | 本次变更摘要 + 操作日志 | 每次归一执行 |
| **角色智库** | 技能→角色映射表 | 角色库有变更时 |
| **设计资产** | 品鉴案例库路径 | 品鉴更新时 |
| **知识库** | 域分类索引 | 批量操作后 |

### Phase 4: 版本一致核验
多源版本交叉验证:
- 启动词系统.html → `v\d+\.\d+`
- 技能库 INDEX.md → 更新时间戳
- 各端 trae-喵老大核心词.txt head 1 → `V\d+\.\d+`
- KG结晶 记录 → 操作日期
- 全部一致 → 完成 | 不一致 → 标记差异表

### Phase 5: 汇报
```
归一同步完成 ✅

五端同步:
  ✅ 部署词 (<部署词>\)
  ✅ Trae家里 (C:\Users\<USER>\.trae-cn\)
  ✅ Trae公司 (C:\Users\<USER>\.trae-cn\)
  ✅ Hermes
  ✅ .trae本地 (<.trae-skills>\)

引用审计:
  ✅ SKILL.md 路径引用 (0处断裂)
  ✅ 域路由一致性
  ✅ INDEX.md 完整性
  ✅ 断链检测

版本一致性:
  启动词系统.html: v9.4
  所有部署文件: v9.4
  技能库 INDEX.md: 2026-06-28

变更记录:
  → KG结晶 已追加
```

## 域前缀命名规范 (审计依据)

| 域 | 前缀 | 示例 |
|----|------|------|
| DEV | `DEV-` | DEV-DEV-cheat-darwin |
| WRITE | `WRITE-` | WRITE-humanizer-zh |
| MEDIA | `MEDIA-` | MEDIA-视频动效-短视频流水线 |
| ART | `ART-` | ART-设计借鉴-品鉴系统(已归档V6.0至备份) |
| WORK | `WORK-` | WORK-办公-办公技能路由 |
| STUDY | `STUDY-` | STUDY-知识库-miao-deep-research |
| THINK | `THINK-` | THINK-认知-消解-miao-cognitive-protocol |
| MEMORY | `MEMORY-` | MEMORY-记忆-凝魂 |
| FINANCE | `FINANCE-` | FINANCE-金融-A股数据 |
| META | `META-` | META-化神 |
| SECURITY | `SECURITY-` | SECURITY-安全审计 |
| ORCHESTRATE | `ORCH-` | ORCH-编排管线 |

**例外保留规则**:
- `通用-*` → 跨域提升技能 (保留原名)
- `外部导入*` → 导入技能 (保留原名)
- `cheat-*` → 评分系统 (保留原名)
- `_archived`, `_规则`, `_系统` → 系统目录 (不处理)

## 踩坑清单 (v1.3更新)

| 坑 | 修复 |
|----|------|
| SSoT路径引用旧版(<技能库>\) | 实际 SSoT 为 <技能库>\ |
| patch工具反斜杠翻倍 | 用 `content.replace()`, 不用patch |
| Hermes活跃profile未同步 | 必须同时复制到 default + profiles\{活跃}\ |
| 旧前缀残留(DEV-,WRITE-) | 全量审计阶段扫描修复 |
| PowerShell中文注释报错 | 用 `powershell -Command` 代替 `-File` |
| 域前缀双重叠加(ART-ART-) | 改名时正则 `^ART-ART-` → `^ART-` |
| 深层嵌套目录 | 提平到一级, 避免 3层以上嵌套 |
| 引用断裂未检测 | Phase 2 全量引用审计扫描所有 SKILL.md |

## 引用 (按需加载)

| 内容 | 路径 |
|------|------|
| 技能库当前索引 | <技能库>\INDEX.md |
| 启动词系统 | <部署词>\启动词系统.html |
| KG结晶变更记录 | <KG结晶>\ |
| 域路由完整版 | META-域路由\SKILL.md |
| 路径一致性审计(旧版) | references/Hermes技能路径一致性审计.md |
| 部署词版本一致性审计 | references/部署词版本一致性审计.md |
| 同步判定逻辑 | references/sync-judgment.md |
| 快捷指令参考 | references/command-reference.md |
| 格式纪律 | references/format-discipline.md |
| 品鉴系统 | ART-设计借鉴-品鉴系统\ (V6.0已归档至备份) |
| 天工优化引擎 | ART-天工\ |

## 版本历史

## OM集成 — 多Agent协作记忆

归一同步状态写入OM，编排器和升仙可读取同步历史。

### 同步完成时写入
```powershell
$summary = "[归一] 同步完成 | 目标: {四端/五端} | 文件: {N}个 | 差异: {M}处 | 状态: {ok/blocked} | $(Get-Date -Format 'yyyy-MM-dd')"
$body = @{content=$summary; tags=@("domain:sync", "source:归一", "scope:global")} | ConvertTo-Json
Invoke-RestMethod -Uri "http://localhost:8080/memory/add" -Method Post -Body $body -ContentType "application/json" -Headers @{"x-api-key"=$env:OM_API_KEY}
```

### 启动时读取 (检查最近同步状态)
```powershell
$body = @{query="归一 同步 状态"; tags=@("domain:sync"); limit=3}
$lastSync = Invoke-RestMethod -Uri "http://localhost:8080/memory/query" -Method Post -Body $body -ContentType "application/json" -Headers @{"x-api-key"=$env:OM_API_KEY}
```

- v1.0 (2026-06-17): 初始版本, 三端同步
- v1.1 (2026-06-22): 管线重构+三端去重+refs落地
- v1.2 (2026-06-28): SSoT修正 + 部署词路径确认
- **v1.3 (2026-06-28): 域前缀架构适配 / 五端同步 / 全量引用审计 / 知识库联动 / 启动词v9.4对齐**
- **v1.4 (2026-07-20): R18愤怒/错误强化态同步 / 鞭子v1.4.0对齐 / 部署词v10.2**

