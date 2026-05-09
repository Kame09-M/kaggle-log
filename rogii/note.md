# ROGII - Wellbore Geology Prediction

## 基本情報
- タスク：回帰
- 評価指標：RMSE（低いほど良い）
- 予測対象：TVT（True Vertical Thickness）
- 締切：2026年6月16日

## データ構造
- 1井戸につき horizontal_well と typewell の2ファイル
- 訓練データ：773井戸・約509万行
- testデータ：3井戸・19221行
- GR（ガンマ線）で地質層を照合してTVTを推定する
- 地質層は10種類（ANCC, ASTNU, ASTNL, EGFDU, EGFDL, BUDA, LBHL, LTHL, LTGT, MNSS）

## EDAで気づいたこと
- GRが29.6%欠損 → 線形補間で対処
- TVT_inputが74.3%欠損 → これが予測対象ゾーン
- ANCC・EGFDLにも一部欠損あり → 中央値で補完
- testデータにはANCC・ASTNU・ASTNL・EGFDU・EGFDL・BUDAが存在しない

## 試したこと
| 手法 | CVスコア | LBスコア | 備考 |
|---|---|---|---|
| LightGBM（TVT_input含む） | 8.724 | 1792.775 | TVT_inputの扱いが問題 |

## 次回やること
- TVT_inputを特徴量から外して再学習する
  → 学習ゾーンではTVT_inputに値があるが評価ゾーンではNaNになるため乖離が発生

## 改善候補リスト
- TVT_inputを特徴量から外す（最優先）
- GR補完方法の改善
  - 先頭・末尾のNaN → ffill/bfillで対処
  - 中央値より井戸ごとの中央値の方が良い可能性あり
- ANCC・EGFDLの欠損補完の改善
- typewellのGRパターンとの照合を特徴量化
- 地質層ラベル（Geology）を特徴量として使う
