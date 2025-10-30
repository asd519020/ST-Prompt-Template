# 怀孕系统安装和测试指南

## 📦 文件清单

您应该有以下文件：
1. `pregnancy_system_worldinfo.json` - 核心系统（第一部分）
2. `pregnancy_system_worldinfo_part2.json` - LLM指导和显示（第二部分）
3. `pregnancy_system_quickreply.json` - 快速回复按钮
4. `PREGNANCY_SYSTEM_INSTALLATION.md` - 本文档

---

## 🔧 安装步骤

### 第一步：导入世界书条目

1. 打开 SillyTavern
2. 点击顶部的 **📚 World Info** 按钮
3. 创建或打开一个 World Info 文件（建议命名为 "Pregnancy System"）
4. 点击右上角的 **⋮ (菜单)** → **Import**
5. 选择 `pregnancy_system_worldinfo.json` 导入
6. 再次点击 **Import**，选择 `pregnancy_system_worldinfo_part2.json` 导入
7. 确认所有条目已成功导入（应该有约10个条目）

### 第二步：启用世界书

1. 在聊天界面，点击顶部的 **📚 World Info** 图标
2. 勾选您刚才创建的 "Pregnancy System" 世界书
3. 确保所有条目都是 **✅ 启用** 状态（绿色勾选）

### 第三步：导入快速回复（可选但推荐）

1. 点击 SillyTavern 界面右下角的 **⚡ Quick Reply** 按钮
2. 点击 **Settings** 图标（齿轮）
3. 点击 **Import** 按钮
4. 选择 `pregnancy_system_quickreply.json` 文件
5. 确认导入成功，您应该看到10个新按钮

### 第四步：配置 Prompt（重要！）

在您的 **System Prompt** 或 **Instruct Mode** 中添加以下代码：

```ejs
<%- getPromptsInjected('labor_guidance') %>
<%- getPromptsInjected('pregnancy_awareness') %>
```

这两行代码会让 LLM 接收到分娩阶段和察觉度的指导信息。

**添加位置示例：**
```
You are a helpful AI assistant.

<%- getPromptsInjected('labor_guidance') %>
<%- getPromptsInjected('pregnancy_awareness') %>

Please respond to the user's messages...
```

---

## 🧪 测试流程

### 基础测试：设置怀孕

1. **方式A：使用快速回复**
   - 点击 `🤰 使角色怀孕` 按钮
   - 输入角色名（例如：`Lily`）
   - 看到提示：`✓ Lily 已怀孕（受孕者：User）`

2. **方式B：直接输入命令**
   - 在聊天框输入：`/pregnant Lily`
   - 发送消息

3. **验证**
   - 点击 `📊 怀孕状态` 查看状态
   - 应该显示：`Lily: 怀孕中（第0天/280天，早期（0-12周））`

### 测试1：察觉度系统（无意识期）

```
1. 设置怀孕：/pregnant Lily
2. 推进时间：点击 ⏭️ 推进时间 → 输入 14 （推进2周）
3. 查看察觉度：/pregnant-awareness Lily
4. 对话测试：
   User: Lily最近怎么样？

   [预期]
   - 状态栏显示：🤷 Lily - 无意识期（察觉度约 4-8%）
   - AI不会提及"怀孕"，只会描述症状（如月经推迟、轻微不适）
```

### 测试2：怀疑期和验孕棒

```
1. 继续推进：/pregnant-advance 28 （共6周）
2. 查看察觉度：应该进入怀疑期（25-49%）
3. 使用验孕棒：/pregnant-test Lily
4. 对话测试：
   User: Lily看到验孕结果后什么反应？

   [预期]
   - AI会根据角色性格描述反应（震惊/冷漠/害怕等）
   - 不会说出具体孕周或预产期（因为还没产检）
```

### 测试3：产检（完全察觉期）

```
1. 产检：/pregnant-doctor Lily
2. 查看状态：/pregnant-awareness Lily
3. 对话测试：
   User: 医生告诉了Lily什么？

   [预期]
   - 状态栏显示：📋 Lily - 完全察觉期（75%+）
   - AI可以提及具体孕周、预产期等信息
   - 根据角色性格自由发挥（冷漠/母性/恐惧等）
```

### 测试4：分娩触发（随机性）

```
1. 跳到预产期：/pregnant-advance 234 （总共280天）
2. 继续对话，观察是否触发分娩
   - 如果运气好（30%概率），会触发产前阵痛
   - 如果没触发，进入过期妊娠状态
3. 继续对话几轮，概率会逐渐增加
4. 或者手动触发：/pregnant-induce Lily
```

### 测试5：完整分娩流程

```
1. 触发分娩：/pregnant-labor Lily
2. 对话推进（每条消息推进0.5小时）：

   [0-3小时] 产前阵痛期
   User: Lily现在感觉如何？
   [预期] AI描述宫缩疼痛，但不会提及破水

   [3-6小时] 第一产程（破水期）
   User: 接下来发生了什么？
   [预期] AI描述破水、宫缩加剧、胎儿下降

   [6-9小时] 第二产程（娩出期）
   User: 继续描述分娩过程
   [预期] AI描述用力、胎儿渐进娩出（头顶→面部→肩膀→身体）

   [9+小时] 分娩完成
   User: 最后的结果？
   [预期] AI描述婴儿完全娩出、角色反应（根据性格）
```

---

## 🎮 所有可用命令

| 命令 | 功能 | 示例 |
|------|------|------|
| `/pregnant <角色名>` | 使角色怀孕 | `/pregnant Lily` |
| `/pregnant-status` | 查看所有怀孕状态 | `/pregnant-status` |
| `/pregnant-advance <天数>` | 推进时间 | `/pregnant-advance 7` |
| `/pregnant-abort <角色名>` | 终止怀孕 | `/pregnant-abort Lily` |
| `/pregnant-labor <角色名>` | 触发分娩 | `/pregnant-labor Lily` |
| `/pregnant-induce <角色名>` | 催产（强制触发） | `/pregnant-induce Lily` |
| `/pregnant-awareness <角色名>` | 查看察觉度 | `/pregnant-awareness Lily` |
| `/pregnant-awareness-add <角色名> <数值>` | 增加察觉度 | `/pregnant-awareness-add Lily 20` |
| `/pregnant-test <角色名>` | 验孕棒测试 | `/pregnant-test Lily` |
| `/pregnant-doctor <角色名>` | 产检 | `/pregnant-doctor Lily` |

---

## 🎭 角色性格测试建议

测试不同性格的角色反应：

### 冷漠杀手型（如 Raven）
```
预期：
- 无意识期：完全忽略症状，专注任务
- 怀疑期：冷淡地思考"麻烦"
- 确认期：面无表情接受事实
- 分娩期：机械地配合，对婴儿无感
```

### 温柔母性型（如 Emma）
```
预期：
- 无意识期：注意到变化但归因于其他
- 怀疑期：既期待又紧张
- 确认期：喜极而泣，开始规划
- 分娩期：虽然痛苦但想着"为了宝宝"
```

### 暴躁战士型（如 Aria）
```
预期：
- 无意识期：烦躁地抱怨身体不适
- 怀疑期：愤怒地否认
- 确认期：咒骂所有人
- 分娩期：大声咒骂，推开他人
```

---

## ⚙️ 高级配置

### 修改配置参数

编辑 World Info 中的 `[InitialVariables] Pregnancy System Config` 条目：

```json
{
  "pregnancy": {
    "config": {
      "autoDetectTime": true,           // 自动检测时间跳跃
      "debug": true,                    // 显示调试信息
      "timeMultiplier": 1.0,           // 时间倍率（2.0=双倍速）
      "hourPerMessage": 0.5,           // 分娩时每条消息推进的小时数
      "baseLaborTriggerChance": 0.3,   // 预产期触发概率（0.3=30%）
      "maxOverdueDays": 14,            // 最大过期天数
      "awarenessGrowthRate": 2         // 察觉度增长速率
    }
  }
}
```

### 调整建议

- **快速测试**：`hourPerMessage: 2` （每条消息推进2小时，加速分娩）
- **高触发率**：`baseLaborTriggerChance: 0.8` （80%触发概率）
- **慢速推进**：`awarenessGrowthRate: 1` （察觉度增长更慢）

---

## 🐛 故障排除

### 问题1：命令无效
**症状**：输入命令后没有反应
**解决**：
1. 确认世界书已启用
2. 检查 `[GENERATE:BEFORE] Pregnancy Command Handler` 条目是否启用
3. 刷新页面重试

### 问题2：状态栏不显示
**症状**：没有看到彩色的状态栏
**解决**：
1. 确认 `[RENDER:AFTER] Pregnancy Status Display` 条目已启用
2. 检查条目中是否有 `@@render_after` 和 `@@message_formatting` 标记

### 问题3：LLM 不遵守指导
**症状**：无意识期角色说"我怀孕了"
**解决**：
1. 确认在 System Prompt 中添加了：
   ```ejs
   <%- getPromptsInjected('pregnancy_awareness') %>
   ```
2. 确认 `[GENERATE:BEFORE] Awareness Guidance` 条目已启用
3. 尝试更明确的角色卡描述角色性格

### 问题4：时间不推进
**症状**：使用 `/pregnant-advance` 后天数没变
**解决**：
1. 使用 `/pregnant-status` 确认角色是否怀孕
2. 检查命令格式：`/pregnant-advance 7`（数字前有空格）
3. 查看浏览器控制台是否有错误

---

## 📝 测试清单

完成以下测试以确保系统正常：

- [ ] 设置角色怀孕
- [ ] 推进时间看到天数变化
- [ ] 察觉度从无意识期→怀疑期→确认期→完全察觉期
- [ ] 使用验孕棒增加察觉度
- [ ] 产检后察觉度大幅提升
- [ ] 到达预产期观察随机触发
- [ ] 过期妊娠后概率递增
- [ ] 手动触发分娩
- [ ] 分娩三阶段正常推进
- [ ] 状态栏正确显示
- [ ] LLM 根据察觉度阶段正确反应
- [ ] LLM 根据角色性格差异化反应

---

## 🎉 测试成功标志

如果您看到以下现象，说明系统运行正常：

✅ 状态栏显示彩色的怀孕/察觉度信息
✅ 命令执行后有反馈消息
✅ 无意识期角色不会说"我怀孕了"
✅ 产前阵痛期不会描述破水
✅ 分娩进度随对话推进
✅ 不同性格角色反应明显不同

---

## 📧 反馈和支持

如果遇到问题或有改进建议，请记录：
1. 使用的 SillyTavern 版本
2. 具体的错误信息（浏览器控制台 F12）
3. 复现步骤
4. 角色卡配置（如果相关）

---

**祝测试愉快！🎉**
