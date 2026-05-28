# Color Dice 游戏需求文档

## 1. 游戏名字

Color Dice

## 2. 未付费玩家限制

未付费玩家下注金额和下注次数有限制，前端需要根据服务下发的下注金额和单局下注次数拦截玩家下注行为。

### a. 未付费玩家下注金额限制

- **i.** 未付费玩家只能下注**【配置值】**（最大最小配置限制），不能拖动拖动条。
- **ii.** `"1/2"`, `"2x"`, `"Min"`, `"Max"` 按钮置灰（禁用状态）。
- **iii.** 点击 `"1/2"` 或 `"Min"` 或输入框输入金额低于最小值时，底部弹出 Toast 提示：

  > The minimum bet amount for un recharge user is `{{.Amount}}`。

- **iv.** 点击 `"2x"` 或 `"Max"` 或输入框输入金额高于最大值时，底部弹出 Toast 提示：

  > The maximum bet amount for un recharge user is `{{.Amount}}`。

### b. 未付费玩家下注次数限制

- **i.** 未付费玩家最多可以在下注区域 bet **【配置值】** 次，超过 **【配置值】** 次数点击下注区域弹出 Toast 提示：

  > Your maximum times of bets is`{{.Amount}}`。

### c. Auto 功能限制

未付费玩家**不能使用 Auto 功能**。

## 3. Payouts 展示

Payouts 取整数，可以将这里的**信息展示位置调整至棋盘上**。

---

## 4. 历史记录 UI 布局调整

历史记录改为**横向排布置顶展示**，一次展示 **5 条记录**。

---

## 5. 下注控制区

### 输入限制

- 默认下注金额为**玩家资产的 1/10**，向下取整到十位
- 低于最小下注配置金额则按**最小下注金额**展示
- 输入框**仅限输入整数**

### 快捷按钮

| 按钮 | 功能 |
|------|------|
| **1/2** | 下注金额减半 |
| **2x** | 下注金额翻倍 |

### 拖动条（下拉箭头展开）

- 滑块支持按**资产区间百分比拖动增长**（增量至少为 **10**）
- **Min 键**：切换最低下注
- **Max 键**：切换当前玩家资产最大值（向下取整到十位）
- 资产**低于最低配置值时不可拖动**
- 下注金额输入框内数值**大于玩家资产**时，拖动条滑块置于 **Max 处**
- 点击**非拖动条区域关闭**

> **UI 效果参考图：** Amount 输入区包含筹码图标、数值输入、1/2 / 2x 快捷按钮、上下箭头、Min 滑块、Max 按钮

---

## 6. 最大下注展示

- 将 Amount 处原 **Max Profit** 修改为 **Max bet**
- 点击 **"!"** 图标展开气泡提示：`Max Bet: NGN xxxxx.xx`（保留两位小数）
- 再次点击 **"!"** 关闭气泡

### 余额不足拦截

确认下注金额后，点击 **bet** 若余额不足，弹出 Toast 提示：

> Balance is not enough.

---

## 7. 游戏音效

添加游戏音效：资源量小，反馈感强烈

---

## 8. 弹窗队列

当玩家触发以下条件时：

```javascript
// 下注时资产不足
BET0 = "bet0"
bridge.emitInsufficientBetBalance(params?: {
  gameId?: string | number;
  direction?: 'horizontal' | 'vertical'
})

// 结算后资产不足
SETTLEMENT0 = "settlement0"
bridge.emitBetMoneyNotEnough(params?: {
  gameId?: string | number;
  direction?: 'horizontal' | 'vertical'
})
```

在游戏内弹出弹窗队列：

- **结算时资产不足** (`settlement0`)、**下注资产不足时** (`bet0`)，屏幕下方弹出 Toast：

  > You balance is not enough.

  然后弹出弹窗队列
