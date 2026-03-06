# Feature Dictionary

## Pipeline 1 / Raw Stock Fields (Straight from Bloomberg)

| Field | Definition | Pipeline usage |
| --- | --- | --- |
| Date | Trading date | raw stock key |
| Ticker | Security identifier | raw stock key |
| Price | Daily close / last price | Bloomberg PX_LAST; later maps to `prc` in Pipeline 3 |
| Open | Daily open price | Bloomberg PX_OPEN; not used by Pipeline 3 models |
| High | Daily high price | Bloomberg PX_HIGH; not used by Pipeline 3 models |
| Low | Daily low price | Bloomberg PX_LOW; not used by Pipeline 3 models |
| TRI_Gross | Gross total return index level | Pulled but not a Pipeline 3 model feature |
| Market_Cap | Daily market capitalization | Later maps to `me` in Pipeline 3 |
| Volume | Daily trading volume | Used directly and inside liquidity features |
| Shares_Out | Shares outstanding | Used directly and inside turnover/growth features |
| P/B | Price-to-book ratio | Raw valuation ratio; also distinct from engineered `bm`/`be_me` |
| P/E | Price-to-earnings ratio | Raw valuation ratio |
| P/S | Price-to-sales ratio | Raw valuation ratio |
| Div_Yield | Dividend yield | Used to build `dy = Div_Yield / 100` |
| Bid_Ask | Average bid-ask spread | Raw liquidity proxy |
| EV | Enterprise value | Raw enterprise-value field |
| EBITDA | EBITDA | Raw profitability field |
| D/E_Ratio | Debt-to-equity ratio | Raw leverage field |
| Vol_30D | Source-provided 30-day volatility | Raw risk feature |
| Vol_90D | Source-provided 90-day volatility | Raw risk feature |
| Assets | Total assets | Raw accounting field; used in many engineered ratios |
| Equity | Total common equity | Used in `bm`, `roeq` |
| Cash | Cash and marketable securities | Used in `cash_ratio`, `salecash` |
| Debt | Debt / liabilities proxy | Used in `lev` |
| Inventory_Flow | Inventory-related flow field | Raw field; carried into Pipeline 3 if present |
| Receivables | Accounts receivable | Used in `salerec` |
| Cur_Assets | Current assets | Used in `currat`, `quick` |
| Cur_Liab | Current liabilities | Used in `currat`, `quick` |
| PPE | Property plant equipment | Used in `tang` |
| Inventory_BS | Balance-sheet inventory | Used in `quick`, `saleinv`, `chinv` |
| Sales | Revenue | Used in `sp`, `sgr`, `salecash`, `saleinv`, `salerec` |
| Net_Income | Net income | Used in `ep`, `roaq`, `roeq`, `acc`, `pctacc` |
| COGS | Cost of goods sold | Raw expense field |
| Capex | Capital expenditure | Raw investment field |
| R&D | Research and development expense | Used in `rd_sale`, `rd_mve` |
| Oper_Inc | Operating income | Raw profitability field |
| Gross_Profit | Gross profit | Used in `gma` |
| Oper_CF | Operating cash flow | Used in `cfp`, `acc` |
| FCF | Free cash flow | Raw cash-flow field |
| ROE_Reported | Source-reported return on equity | Raw quality field |
| ROA_Reported | Source-reported return on assets | Raw quality field |
| Oper_Margin | Source-reported operating margin | Raw quality field |
| Net_Margin | Source-reported net margin | Raw quality field |
| Sector | Industry sector | Not part of current Pipeline 3 numeric model features |
| Employees | Employee count | Not part of current Pipeline 3 numeric model features |
| Name | Security name | Not part of current Pipeline 3 numeric model features |

## Pipeline 1 / Raw Macro Fields from Current Default Config

| Macro field       | Definition                  |
| ----------------- | --------------------------- |
| US_RiskFree_3M    | US 3M risk-free proxy       |
| US_Bond_10Y       | US 10Y yield                |
| US_Market_SP500   | US equity market index      |
| US_Volatility_VIX | US implied volatility proxy |
| Comm_Brent_Oil    | Brent oil price             |
| Comm_Gold_Spot    | Gold spot price             |
| Comm_Copper       | Copper price                |
| USD_VND_FX        | USD/VND FX rate             |
| VN_Market_Index   | Vietnam market index        |
| US_Dollar_Index   | Dollar index proxy          |


## Pipeline 2 Engineered Stock Features

| Feature         | Formula                                        | Interpretation                                                           |
| --------------- | ---------------------------------------------- | ------------------------------------------------------------------------ |
| bm              | Equity / Market_Cap                            | Pipeline 2 engineered value ratio; later copied to Pipeline 3 as `be_me` |
| ep              | Net_Income / Market_Cap                        | earnings-to-price style feature                                          |
| cfp             | Oper_CF / Market_Cap                           | cash-flow-to-price style feature                                         |
| sp              | Sales / Market_Cap                             | sales-to-price style feature                                             |
| dy              | Div_Yield / 100                                | dividend yield converted from percent to decimal                         |
| lev             | Debt / Assets                                  | leverage                                                                 |
| cash_ratio      | Cash / Assets                                  | cash intensity                                                           |
| roaq            | Net_Income / Assets                            | return on assets proxy                                                   |
| roeq            | Net_Income / Equity                            | return on equity proxy                                                   |
| gma             | Gross_Profit / Assets                          | gross profitability                                                      |
| currat          | Cur_Assets / Cur_Liab                          | current ratio                                                            |
| quick           | (Cur_Assets - Inventory_BS) / Cur_Liab         | quick ratio                                                              |
| tang            | PPE / Assets                                   | asset tangibility                                                        |
| salecash        | Sales / Cash                                   | sales-to-cash ratio                                                      |
| saleinv         | Sales / Inventory_BS                           | sales-to-inventory ratio                                                 |
| salerec         | Sales / Receivables                            | sales-to-receivables ratio                                               |
| rd_sale         | R&D / Sales                                    | R&D intensity vs sales                                                   |
| rd_mve          | R&D / Market_Cap                               | R&D intensity vs market value                                            |
| mom1m           | pct_change(Price, 21)                          | 1-month momentum proxy using 21 trading days                             |
| mom6m           | pct_change(Price, 126)                         | 6-month momentum proxy                                                   |
| mom12m          | pct_change(Price, 252)                         | 12-month momentum proxy; later copied to Pipeline 3 as `ret_12_1`        |
| mom36m          | pct_change(Price, 756)                         | 36-month momentum proxy                                                  |
| chmom           | mom6m - lag21(mom6m)                           | change in 6-month momentum                                               |
| agr             | pct_change(Assets, 252)                        | asset growth                                                             |
| sgr             | pct_change(Sales, 252)                         | sales growth                                                             |
| chcsho          | pct_change(Shares_Out, 252)                    | share issuance / dilution proxy                                          |
| chinv           | pct_change(Inventory_BS, 252)                  | inventory growth                                                         |
| pchsale_pchinvt | sgr - chinv                                    | sales growth net of inventory growth                                     |
| age             | cumcount(Ticker) / 252                         | listing-age style proxy in years                                         |
| maxret          | rolling max of daily Price pct_change over 21  | short-horizon extreme-return proxy                                       |
| retvol          | rolling std of daily Price pct_change over 252 | 1-year realized volatility                                               |
| idiovol         | rolling std of daily Price pct_change over 21  | 1-month realized volatility proxy                                        |
| turn            | Volume / Shares_Out                            | share turnover                                                           |
| std_turn        | rolling std of turn over 252                   | turnover volatility                                                      |
| acc             | (Net_Income - Oper_CF) / Assets                | accruals proxy                                                           |
| absacc          | abs(acc)                                       | absolute accruals                                                        |
| pctacc          | (Net_Income - Oper_CF) / abs(Net_Income)       | scaled accruals                                                          |

## 6) Pipeline 2 Processing-stage Daily Fields

| Field | Formula | Meaning |
| --- | --- | --- |
| dollar_vol | Price * Volume | daily dollar volume |
| adv_med | rolling median(dollar_vol, liq_win) | median dollar volume liquidity proxy |
| ret_1d_full | groupby(Ticker).Price.pct_change() | temporary full-calendar daily return |
| y_next_1d_raw_full | groupby(Ticker).shift(-1, ret_1d_full) | temporary next-day target on full calendar |
| ret_1d | ret_1d_full retained after tradability screen | daily return kept in cleaned stock data |
| y_next_1d_raw | y_next_1d_raw_full retained after tradability screen | unclipped daily target kept in cleaned stock data |
| y_clip_flag | abs(y_next_1d_raw) > target_clip | diagnostic bool flag; not a final Pipeline 3 model feature |
| y_next_1d | clip(y_next_1d_raw, -target_clip, +target_clip) | daily clipped target used for Pipeline 2 downstream monthly compounding |
| ret_outlier_flag | abs(ret_1d) > outlier_abs_ret_flag | diagnostic bool flag; not a final Pipeline 3 model feature |

---
*Notes:*
- Pipeline 2 produces daily cleaned stock data plus engineered daily features.
- Pipeline 3 does **not** model those daily rows directly. It first converts them to a **monthly panel** and then applies another preprocessing layer.
- The aligned interpretation is therefore: **raw fields -> Pipeline 2 engineered daily fields -> Pipeline 3 monthly panel fields -> Pipeline 3 final model feature set**.
- Current Pipeline 3 artifacts contain **112 panel columns** and **105 final model features**.
---

## Pipeline 3 Monthly Panel Contract

| Field          | Formula / source                                  |
| -------------- | ------------------------------------------------- |
| id             | Ticker                                            |
| eom            | month_end(Date)                                   |
| prc            | Price                                             |
| me             | Market_Cap                                        |
| ret            | compound_return(y_next_1d within each Ticker-eom) |
| rf_1m          | compound_return(DGS3MO / 100 / 252 within month)  |
| ret_exc        | ret - rf_1m                                       |
| ret_exc_lead1m | groupby(id).shift(-1, ret_exc)                    |
| be_me          | bm                                                |
| ret_12_1       | mom12m                                            |

## Pipeline 3 Final Preprocessing Before Model Fit

Pipeline 3 models do not consume raw monthly values directly. `preprocess.py` applies the following:

1. Filter rows with `prc >= 5` and `me >= 25`. ==This is currently still wrong as the currency in VN is much big, thus liquidity is still a problem==
2. Drop rows missing `ret_exc_lead1m`.
3. Drop very sparse columns below the coverage threshold, while forcing required columns to remain.
4. Fill numeric missing values by monthly cross-sectional median. ==this is currently repetitive in pipeline 2, will need to fix later==
5. Create `ret_lead1m = groupby(id).shift(-1, ret)` for portfolio performance calculations.
6. Infer `rf_1m = ret - ret_exc` monthly inside preprocessing.
7. Define `feature_cols` as numeric, non-boolean columns excluding reserved helper columns.
8. Winsorize feature columns cross-sectionally by month at the configured lower/upper quantiles.
9. Rank-scale each feature by month to the interval `[-1, 1]`.
## Final Pipeline 3 Feature Universe

### Required core predictors used by Pipeline 3

- me
- be_me
- ret_12_1
- rf_1m

### Raw stock / accounting fields that survive into final feature_cols

- Assets
- Bid_Ask
- COGS
- Capex
- Cash
- Cur_Assets
- Cur_Liab
- D/E_Ratio
- Debt
- Div_Yield
- EBITDA
- EV
- Equity
- FCF
- Gross_Profit
- Inventory_BS
- Inventory_Flow
- Market_Cap
- Net_Income
- Net_Margin
- Oper_CF
- Oper_Inc
- Oper_Margin
- P/B
- P/E
- P/S
- PPE
- Price
- R&D
- ROA_Reported
- ROE_Reported
- Receivables
- Sales
- Shares_Out
- Vol_30D
- Vol_90D
- Volume

### Engineered stock features used by Pipeline 3

- absacc
- acc
- adv_med
- age
- agr
- bm
- cash_ratio
- cfp
- chcsho
- chinv
- chmom
- currat
- dollar_vol
- dy
- ep
- gma
- idiovol
- lev
- maxret
- mom12m
- mom1m
- mom36m
- mom6m
- pchsale_pchinvt
- pctacc
- quick
- rd_mve
- rd_sale
- retvol
- roaq
- roeq
- salecash
- saleinv
- salerec
- sgr
- sp
- std_turn
- tang
- turn

### Macro / regional / external series observed in actual Pipeline 3 inputs

- China_Shanghai_Index
- Comm_Brent_Oil
- Comm_Copper
- Comm_Gold_Spot
- Comm_Natural_Gas
- Free_Float_Pct
- Global_Baltic_Dry
- Hong_Kong_Index
- Indonesia_Index
- Philippines_Index
- Textile_Cotton_Price
- Thailand_Index
- USD_CNY_FX
- USD_VND_FX
- US_Bond_10Y
- US_CPI_YoY
- US_Dollar_Index
- US_FedFunds_Rate
- US_GDP_QoQ
- US_Market_SP500
- US_RiskFree_3M
- US_Volatility_VIX
- VN_CPI_YoY
- VN_Market_Index
- VN_MoneySupply_M2
## Model Usage Details

| Model | Feature set used                      | Usage detail                                                            |
| ----- | ------------------------------------- | ----------------------------------------------------------------------- |
| OLS   | all 105 final numeric feature columns | uses full Pipeline 3 feature set after winsorization and rank scaling   |
| ENET  | all 105 final numeric feature columns | same feature universe as OLS, then regularized selection by Elastic Net |
| PLS   | all 105 final numeric feature columns | same feature universe, reduced to latent components                     |
| PCR   | all 105 final numeric feature columns | same feature universe, reduced by principal components                  |
| GBRT  | all 105 final numeric feature columns | same feature universe; tree model chooses splits nonlinearly            |
| RF    | all 105 final numeric feature columns | same feature universe; ensemble tree model                              |
| OLS3  | [me, be_me, ret_12_1]                 | fixed three-feature model from config, not the full feature universe    |
