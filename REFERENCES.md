# 参考记录：WorkBuddy-Daily（备选，未安装）

> 记录日期：2026-10-16。用途：今后若需要「app 内各种积分任务全自动执行」时，直接参照本条，无需重新调研。

## 项目信息

- 仓库：https://github.com/L0NE-6/WorkBuddy-Daily
- Star：305（2026-10-16 查）
- 形态：单文件自包含 Python 脚本（仅依赖 requests），青龙面板 / GitHub Actions / 本地 python 均可跑
- 定位：WorkBuddy 成长中心全能签到脚本

## 能力覆盖（对应当前技能的缺口）

| 能力 | 本仓库补丁版技能 | WorkBuddy-Daily |
|---|---|---|
| Buddy加油站签到 | ✅ | ✅ |
| 派猫猫旅行闭环 | ✅ | ✅ |
| 积分总余额 | ✅（3.1.8 补丁） | ✅ |
| 成长任务自动执行+领奖 | ❌ 仅只读展示 | ✅ 17 项云端任务 |
| 小程序链式任务 | ❌ | ✅ 7 项（需 `X-Client-Platform: miniprogram` 头；完成对话/选专家/建定时任务/用 GLM5.2，每项 +100~300 积分，链式次日零点解锁） |
| 互动玩法 | ❌ | ✅ 8 项 |
| 多账号 / 青龙 / Actions | ❌ | ✅ |
| 消息推送 | ✅ 12 渠道 | ✅ 多渠道 |

## 部署方式（需要时）

1. 青龙面板：上传 `workbuddy_daily.py`，设变量 `WORKBUDDY_REFRESH_TOKEN`（每行一个 `手机号:AT:RT`），cron 例 `0 7,12 * * *`
2. 本地：`python workbuddy_daily.py`
3. refresh_token 可从本仓库补丁版技能导出：`python scripts/buddy_station.py --export-rt`（通道 C），两者凭据体系通用（同账号同域）
4. Releases 页有完整包 zip + SHA256 校验

## 用户决策记录（2026-10-16）

用户明确：**暂不安装**。当前只需猫猫旅行 + 签到，其他积分任务手动随时做。本条仅存档。

## 同生态其他参考（按 star）

| 项目 | Star | 特点 |
|---|---|---|
| jlcodes99/cockpit-tools | 19017 | 账号 OAuth/签到模块 |
| yetone/magpie | 13468 | 签到 + 余额读取 |
| edison7009/EchoBird | 3307 | 积分工作台 |
| changexbc/workbuddy-switch | 1305 | 积分查询/切换 |
| 88lin/workbuddy-auto-signin | 1279 | 自动签到 |
| Cat-zaizai/ZaiZaiCat-Checkin | 486 | 签到+旅行 |
