# workbuddy-score-patched

**WorkBuddy积分助手 3.1.8 本地补丁版** —— 基于技能市场原作 [totorosir-workbuddy-score](https://github.com/)（TOTORO / 公众号「龙猫科技说」）v3.1.7 的修订 fork。

## 补丁内容（唯一变更点）

修正「积分余额」字段映射错误：

- **问题**：原作把 `checkin-activity-status` 接口的 `total_credits` 当作「积分余额（总积分）」播报。该字段实为 **Buddy加油站本期活动累计**（例：13 天 × 100 分/天 = 1300），与 app 内显示的账户总余额（例：2,567.44）口径不同，数字偏小。
- **修复**：新增 `fetch_credit_balance()`，调 `POST /billing/meter/get-user-resource-summary`（**无 /v2 前缀**）取真实总余额。关键细节：
  - 该接口有 WAF：必须带 `Origin` + `Referer: /profile/growth-center` + Electron UA（`WorkBuddy/5.5.4 ... Electron/37.10.3`）三件套，否则 403 code 10085「请求不合法」
  - 总余额 = `data.Packages[].CycleRemainCapacity` 求和（float 小数计量，AI 对话按小数扣点）
- **播报变更**：文字 / JSON / HTML / 系统通知 / 推送均改为「积分总余额」+「Buddy加油站本期累计」双行；`web_auth.py`（通道 B）三处同源误标同步修正
- **不改动**：签到、派猫猫旅行（先领后派闭环）、成长计划只读展示、三通道授权等全部原有逻辑；写操作仍仅限三个已验证接口

降级行为：余额接口失败时该行显示「暂不可用（接口受限）」，不影响签到与旅行闭环。

## 版本说明

本地版本号 **3.1.8**（高于市场版 3.1.7）。WorkBuddy 客户端技能同步逻辑判定「本地版本/内容领先 → local_ahead 本地赢」，升版 + 内容差异可防止被市场自动更新覆盖。若技能市场提示此技能有更新，请勿点击。

## 使用

```bash
# 每日播报（只读）
python scripts/buddy_station.py

# 自动签到（幂等）
python scripts/buddy_station.py checkin

# 签到 + 派猫猫旅行全自动闭环（先领后派，显式开关）
python scripts/buddy_station.py checkin --travel-auto

# 机器可读 JSON
python scripts/buddy_station.py --json
```

依赖：Python 3.8+ 纯标准库，零第三方包。授权：读取本机 WorkBuddy 登录态（通道 A 零配置；通道 B 一次性网页授权；通道 C 长效 refresh_token）。详见 `SKILL.md`。

## 参考实现（余额接口取证来源）

- [L0NE-6/WorkBuddy-Daily](https://github.com/L0NE-6/WorkBuddy-Daily)（305★）
- [steipete/CodexBar](https://github.com/steipete/CodexBar)（22.5k★，cookie 会话 + 同款 Origin/Referer）
- [smart-open/TraeWorkAssistant](https://github.com/smart-open/TraeWorkAssistant)（293★）

## 目录

```
SKILL.md                技能指令正文（含版本说明）
scripts/buddy_station.py 主脚本（播报/签到/旅行闭环）
scripts/web_auth.py      通道 B 网页授权
scripts/rt_auth.py       通道 C 长效令牌
scripts/push_notify.py   12 渠道消息推送
references/              接口明细与输出样例
templates/               授权片段模板
```

原版 3.1.7 未改动备份随用户工作区保存；本仓库即补丁版权威副本。
