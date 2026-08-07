# A股市场情绪日报网页

公开网址：https://qrq3838.github.io/a-share-market-sentiment-dashboard/

## 每日盘后更新

服务器项目根目录为：

`F:\res4\rqqin\strategy research\crowding`

每天盘后可直接双击项目根目录中的 `盘后检查并更新网页.cmd`。也可以在 PowerShell 的任意目录运行：

```powershell
& "F:\res4\rqqin\strategy research\crowding\site\DailyCheckAndUpdate.ps1"
```

程序会依次完成：

1. 增量检查 Wind 市场、四个指数、31个申万一级行业和成交额前5%个股数据。
2. 分别核对八个米筐基准指数、总量融资余额、A股流通市值、个股融资余额、行业融资数据、data_interface行业PB底表和各网页快照的最新日期。
3. 只对落后的数据源发起增量查询；已经最新的模块跳过查询，不重复下载全部历史或当年全年数据。
4. 重建需要更新的衍生指标，校验日期覆盖、指数、31个行业、融资数据、行业PB及其全历史分位的完整性。
5. 校验通过后生成单页 `index.html`，其中包含行业PB横截面离散度，以及可切换31个申万一级行业的每日整体PB与全历史分位曲线；随后提交并推送到 GitHub Pages。
6. 输出每个模块的最新日期、状态和本次米筐额度用量。

如果盘后米筐或 data_interface 尚未发布当日数据，程序会保留最后一个已有完整交易日，不会把空白数据写进网页；稍后重新运行即可自动补齐。

完整日志位于 `F:\res4\rqqin\strategy research\crowding\logs`，最近一次检查结果为 `freshness-latest.json`。异常时 GitHub 上原有网页不会被覆盖。

仅在确实需要重建全部历史时使用：

```powershell
& "F:\res4\rqqin\strategy research\crowding\site\DailyCheckAndUpdate.ps1" -ForceFull
```

## 更新口径

- Wind 日常更新重查最近7个自然日，以覆盖延迟修订。
- 当天16:30之前不把当天视为候选完整交易日。
- 网页仅使用四个指数、行业和个股成交聚合均完整的交易日。
- 行业PB来源于 `data_interface`：行业内有效成分股总市值之和除以同一批股票在当时最新可得的归母净资产之和；财报于公告日后的下一交易日生效，负PB保留。
- 行业PB分位采用全历史口径：每个交易日的PB都在该行业当前完整历史（包括该日之后的观测）中计算平均秩百分位；新增交易日后会基于扩展后的完整历史重新计算整段序列。
- 不上传原始工作簿、数据库登录信息、服务器私钥或完整个股历史明细。
- 页面仅供研究交流，不构成投资建议。
