# ROGII - Wellbore Geology Prediction

## 基本情報
- タスク：回帰
- 評価指標：RMSE（低いほど良い）
- 予測対象：TVT（True Vertical Thickness）
- 締切：2026年6月16日

## データ構造
- 1井戸につき horizontal_well と typewell の2ファイル
- 訓練データ：773井戸・約509万行
- GR（ガンマ線）で地質層を照合してTVTを推定する

## EDAで気づいたこと
- GRが29.6%欠損 → 線形補間で対処予定
- TVT_inputが74.3%欠損 → これが予測対象ゾーン
- 地質層は10種類（ANCC, ASTNU, ASTNL, EGFDU, EGFDL, BUDA, LBHL, LTHL, LTGT, MNSS）

## 前処理方針
- GR欠損 → 線形補間（前後の値を直線で結ぶ）

## 試したこと
| 手法 | CVスコア | LBスコア |
|---|---|---|
| （まだなし） | - | - |

## 改善候補リスト
- GR補完方法の検討
- typewellのGRパターンとの照合を特徴量化
- 地質層ラベル（Geology）を特徴量として使う
