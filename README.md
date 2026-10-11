# 猜英雄 · 线索阶梯（LOL Guess）

英雄联盟猜英雄小游戏——通过 8 级渐进线索一步步推理出神秘英雄，内置 173 位英雄的中文档案与图鉴。

**在线游玩：[https://www.lolguess.top](https://www.lolguess.top/)**

## 玩法

### 线索阶梯（主模式）
- 每局随机抽取一位英雄，从模糊到具体共 8 级线索（称号 → 阵营 → 分路 → 技能 → 台词……）
- 每解锁一条新线索都会消耗一次机会，越早猜中得分越高
- 支持按名字、拼音、英文 ID、称号搜索英雄

### 属性对决（Loldle 式）
- 对比 7 个属性维度：阵营、职业、分路、伤害类型、攻击距离、资源、上线区域
- 每个属性给出方向提示（相同 / 接近 / 不同），二分收敛锁定答案
- 猜测次数可分级设置：

| 档位 | 次数 |
|---|---|
| 标准 | 6 次（默认） |
| 轻松 | 10 次 |
| 宽松 | 15 次 |
| 训练 | 20 次 |
| 无限 | 无上限（不计入总战绩，单独统计） |

### 英雄图鉴
- 173 位英雄完整中文档案：种族、阵营、职业、分路、伤害类型、攻击距离、上线年份、售价、皮肤数量、QWER 技能、台词、玩家常称外号等
- 每位英雄独立静态页面，支持搜索引擎收录

## 特点

- **纯前端、离线可玩**：运行时零外部请求，数据全部内置于构建产物
- **SEO 友好**：预渲染 177 个静态 HTML 页面，含 canonical / sitemap / robots
- **玩家外号搜索**：支持"剑魔""诺手""月男"等 168 组玩家简称直接命中
- **数据可同步**：内置脚本可从 Riot 官方 Data Dragon 拉取最新数据

## 技术栈

Vite · React 18 · TypeScript · Tailwind CSS 3.4

## 仓库说明

本仓库 `main` 分支为 **GitHub Pages 部署产物**（预渲染静态站点），由本地源码项目通过构建脚本生成，包含：

```
index.html          首页
game/               游戏入口（React 应用）
heroes/             173 位英雄图鉴页
sitemap.xml         站点地图
CNAME               自定义域名（www.lolguess.top）
```

## 数据维护

英雄数据快照来源于 [Riot Games Data Dragon](https://ddragon.leagueoflegends.com/)，同步口径记录在 `provenance.json`。本地源码项目提供以下构建期脚本：

```bash
npm run data:fetch    # 拉取官方最新数据，输出差异报告（默认 dry-run，不写入）
npm run data:apply    # 确认后写回本地数据文件
npm run validate:data # 数据完整性校验（5 项检查）
npm run test:smoke    # 冒烟测试（194 项断言）
npm run build:pages   # 构建 + 重新生成全部静态页面
```

数据同步只自动更新数值字段（皮肤数量、攻击距离、难度），台词、外号、标签等中文文案字段始终人工维护。

## 许可

游戏数据版权归 Riot Games 所有，本项目仅为非营利粉丝作品，与 Riot Games 无官方关联。

## 访问量与游玩数据分析

站点为纯静态 GitHub Pages，GitHub 不提供访问日志，玩家数据默认只存在浏览器本地。要集中查看访问量与游玩趋势，可接入轻量分析工具：

- 配置位于 `site.config.json` 的 `analytics` 字段，**默认 `enabled: false`**（不注入任何外部脚本，保持运行时零请求）。
- 支持两种 provider：**GoatCounter**（免费托管，推荐）与 **Umami**（可自托管，数据完全自控）。
- 启用步骤：① 到对应平台注册并拿到站点代码；② 填 `code`、`provider`，必要时填 Umami 的 `endpoint`；③ 把 `enabled` 改为 `true`；④ 重新执行 `npm run build:pages`。
- 自动收集：页面访问量（PV/UV、来源、设备、热门页面）+ 游戏事件（`round_start` 开局、`round_end` 结算，含模式 / 档位 / 结果 / 用掉的猜测次数 / 答案英雄）。
- 事件经 `src/lib/analytics.ts` 上报；该模块在 `enabled: false` 时为空操作，离线或被拦截时静默失败，不影响游戏。

> 注：本项目用户主要在境内，GoatCounter 服务器在境外，可能存在访问延迟；若需境内稳定或数据完全自控，建议改用自托管 Umami（可放在同厂商轻量服务器）。
