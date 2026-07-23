[Readme.txt](https://github.com/user-attachments/files/30316899/Readme.txt)
# Stock Return Forecasting and Portfolio Construction Using an Event-Conditional Mixture-of-Experts Framework

One notebook (`Article_code.ipynb`). Functions, by section:

## 1. Libraries
Imports only.

## 2. Import data
- `fetch_price` — downloads one ticker's daily OHLCV from Yahoo Finance.
- `read_price_excel` — reads one ticker's saved prices from `Initial stock data/`.
- `load_price` — returns one ticker's prices from memory (download or Excel).
- `import_prices` — loads all ticker price series and, when downloading, saves them to `Initial stock data/`.
- `load_events` — reads the FOMC/CPI/NFP macro-event calendar.

## 3. Other functions
- `load_predictions` — returns one ticker/model/horizon prediction from the in-memory store.
- `vol_model_features` — returns one ticker's GARCH/HAR volatility features.
- `load_saved_predictions` — loads all saved predictions from `Final predictions/` into memory.
- `technical` — builds six technical features from prices.
- `macro_features` — builds six macro-event features (days-to-event, is-event).
- `compute_vol_models` — one-step GARCH(1,1) and HAR-RV volatility forecasts.
- `build_features` — assembles all features and the forward-return target for one ticker.
- `make_windows` — turns features into fixed-length input windows and targets.
- `standardize_windows` — z-scores the windows using train-window statistics only.
- `capacity` — returns a network's layer widths and attention heads.
- `phi` — the window descriptor (mean and std) used for regime similarity.
- `portfolio_metrics` — annualised return, volatility, Sharpe, Sortino, Calmar, drawdown, hit rate.
- `score_price_panels` — builds the forecast-score and price panels for a portfolio.
- `min_weight` — the minimum position size for the all-names methods.
- `apply_floor` — drops sub-minimum weights and renormalises.
- `method_weights` — target weights for a weighting method on a rebalance date.
- `rebal_dates` — rebalance dates for each method; `EqualWeight_BuyHold` is allocated once and then allowed to drift.
- `backtest` — daily, cost-aware backtest of a weighting rule.
- `build_portfolio` — a book's net returns, horizon-rebalanced 1/N benchmark, metrics and config.
- `trade_ledger` — share-level long-only ledger (trades, positions, PnL, fees).
- `build_and_save` — builds a portfolio book and caches it in memory.
- `sweep_real` — every book, ranked by Sharpe.
- `sharpe_test_portfolio_real` — Jobson-Korkie/Memmel Sharpe test of a book vs its horizon-rebalanced 1/N benchmark.
- `dm_pred` — the Diebold-Mariano statistic and p-value for a loss differential.
- `model_pred_summary` — mean forecast metrics for one model across all tickers.
- `dm_predictions_real` — Diebold-Mariano test of each model against NAIVE, per ticker.
- `window_labels` — labels each date by calendar year.
- `walkforward_stability` — cross-sectional IC per year and its stability summary.

## 4. Models
- `patience` — early-stopping patience scaled to the epoch budget.
- `build_keras` — builds the network (single LSTM, or the MoE / MoE4 mixture of experts).
- `keras_train_predict` — trains one network and returns its test-set forecast.
- `keras_fit` — trains one network on given windows (used by the regime models).
- `regime_blend` — Event-MoE-Regime: one expert per period, blended by similarity (per ticker).
- `ensure_regime_pooled` — trains the pooled per-period experts on all tickers (Regime-Pooled).
- `regime_blend_pooled` — predicts one ticker from the pooled Regime-Pooled experts.
- `ensure_regime_pooled_aug` — trains the pooled four-expert experts with GARCH/HAR (Regime-Pooled-Aug).
- `regime_blend_pooled_aug` — predicts one ticker from the Regime-Pooled-Aug experts.
- `arima_predict` — leak-free ARIMA forecast on daily log-returns.
- `train_model` — trains (or loads) one ticker/model/horizon and stores the prediction.

## 5. Train
- `train_all` — trains every ticker, model and horizon.
- `write_predictions` — writes each ticker's predictions to `Final predictions/<TICKER>_preds.xlsx`.

## 6. Portfolios
- `build_books` — builds every model × horizon × method × K book.

## 7. Analysis
- `accuracy_table` — mean accuracy metrics per model and horizon.
- `per_asset_ic_table` — per-ticker IC and directional accuracy.

## 8. Figures
- `book_sharpe` — a book's Sharpe (or None if missing).
- `fig_ic` — Figure 1, mean IC by model and horizon.
- `fig_equity` — Figure 2, equity curves at the monthly horizon.
- `fig_sharpe_k` — Figure 3, Sharpe vs number of names held (K).
- `fig_best_growth` — Figure 4, capital trajectory of the best book vs 1/N.
- `fig_ic_sharpe` — Figure 5, IC vs portfolio Sharpe scatter.
- `fig_sharpe_heat` — Figure 6, Sharpe by model and method.
- `fig_wf_ic` — Figure 7, cross-sectional IC by year.
- `fig_ic_heat` — Figure 8, per-stock IC across models.
- `fig_pnl_bar` — Figure 9, per-name net PnL for the best book.
