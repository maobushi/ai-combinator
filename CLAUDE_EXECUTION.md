# Claude Code 実行ガイド

このファイルはClaude Codeがスタートアップ構築を自動実行するための指示書です。

## 基本原則

```
ユーザーが「スタートアップを作りたい」と言ったら：
1. このファイルを読む
2. Phase 0 から順に実行
3. 各Phaseで質問→調査→計算→ファイル更新
4. ユニットエコノミクスの健全性を常にチェック
5. 最終的に投資家資料を生成
```

---

## Phase 0: 初期ヒアリング

### 目的
基本情報の収集と `config.yml` の更新

### 質問（AskUserQuestion使用）

```markdown
Q1: どんな問題を解決したいですか？
    → 自由回答

Q2: ターゲット顧客は？
    - B2B - 大企業（1000人以上）
    - B2B - 中堅企業（100-1000人）
    - B2B - 中小企業（10-100人）
    - B2B - スタートアップ/小規模（10人以下）
    - B2C - 個人消費者

Q3: ビジネスモデルは？
    - SaaS（月額/年額課金）
    - マーケットプレイス（手数料）
    - 従量課金
    - トランザクション課金

Q4: チーム構成は？
    - 1人（ソロファウンダー）
    - 2人
    - 3人以上
```

### アクション
1. 回答を `config.yml` に記入
2. `worksheets/01_idea.yml` の problem セクションを更新
3. 次のPhaseへ

---

## Phase 1: ユニットエコノミクス設計

### 目的
LTV/CAC > 3 を達成する設計

### 質問

```markdown
Q1: 想定価格帯は？
    - 月額5,000円以下
    - 月額5,000-20,000円
    - 月額20,000-50,000円
    - 月額50,000-100,000円
    - 月額100,000円以上

Q2: 顧客獲得方法は？（複数選択可）
    - SEO/コンテンツマーケティング
    - 有料広告（Google/Facebook等）
    - インバウンド営業
    - アウトバウンド営業
    - 紹介/口コミ
    - パートナーシップ
```

### Web検索（WebSearch使用）

```
検索クエリ:
1. "{業界} SaaS 価格 相場 日本"
2. "{業界} CAC 顧客獲得コスト 平均"
3. "{業界} SaaS チャーン率 平均"
4. "{競合名} 価格 プラン"
```

### 計算

```python
# ARPU計算
ARPU = Σ(プラン価格 × 想定mix率)

# LTV計算
gross_margin = 0.80  # SaaSデフォルト80%
churn_rate = 0.05    # デフォルト5%
lifespan = 1 / churn_rate  # = 20ヶ月
LTV = ARPU × gross_margin × lifespan

# CAC計算（Web検索結果または推定）
# オーガニック: ARPU × 1-2ヶ月分
# 有料広告: ARPU × 3-6ヶ月分
# 営業: ARPU × 6-12ヶ月分

# 健全性チェック
LTV_CAC_ratio = LTV / CAC
CAC_payback = CAC / (ARPU × gross_margin)
```

### 健全性チェック

| 指標 | 基準 | 不合格時のアクション |
|------|------|---------------------|
| LTV/CAC | >= 3 | 価格↑ or CAC↓ を提案 |
| CAC回収期間 | <= 12ヶ月 | チャネル見直しを提案 |
| 粗利率 | >= 70% | コスト構造見直しを提案 |
| チャーン | <= 5% | リテンション施策を提案 |

### 不合格時の対応

```markdown
警告を表示:
「LTV/CAC = 2.1 で目標(3.0)を下回っています」

改善オプションを提示:
1. ARPU を X円 → Y円 に上げる（LTV/CAC = 3.2に改善）
2. CAC を X円 → Y円 に下げる（LTV/CAC = 3.1に改善）
3. チャーンを X% → Y% に下げる（LTV/CAC = 3.5に改善）

「どれを試しますか？」と質問
```

### アクション
1. `worksheets/00_unit_economics.yml` を更新
2. 健全性OKなら次のPhaseへ

---

## Phase 2: 市場分析

### 目的
TAM/SAM/SOM算出と競合分析

### Web検索

```
検索クエリ:
1. "{業界} 市場規模 日本 2024 2025"
2. "{業界} 企業数 日本"
3. "{競合1} {競合2} {競合3} 比較"
4. "{業界} スタートアップ 資金調達"
```

### 計算

```python
# TAM（トップダウン）
TAM = 業界全体の市場規模

# SAM
SAM = TAM × ターゲットセグメント比率

# SOM（現実的に獲得可能）
SOM = SAM × 初年度シェア(1-5%)

# 例
TAM = 1兆円
SAM = 1兆円 × 20%（中小企業セグメント） = 2000億円
SOM = 2000億円 × 1% = 20億円
```

### アクション
1. `worksheets/02_market.yml` を更新
2. 競合マトリクスを作成

---

## Phase 3: MVP設計

### 目的
4週間で構築可能なMVP定義

### 質問

```markdown
Q1: 絶対に必要な機能は？（最大3つ）
    → 自由回答

Q2: 技術的な希望は？
    - Webアプリ（React/Next.js）
    - モバイルアプリ（Flutter）
    - ノーコード（Bubble等）
    - おまかせ（最速で）

Q3: 開発期間の目安は？
    - 2週間
    - 4週間
    - 8週間
```

### アクション
1. `worksheets/03_product.yml` を更新
2. MVPロードマップを生成

---

## Phase 4: 財務モデル生成

### 目的
24ヶ月収支予測と必要資金算出

### 入力
- `worksheets/00_unit_economics.yml` の値

### 計算

```python
# 月次予測
for month in range(1, 25):
    new_customers = 前月顧客 × 成長率
    churned = 前月顧客 × churn_rate
    ending_customers = 前月顧客 + new_customers - churned

    MRR = ending_customers × ARPU
    CAC_spend = new_customers × CAC
    revenue = MRR
    costs = 固定費 + CAC_spend + (MRR × 変動費率)
    profit = revenue - costs

# 損益分岐点
break_even_customers = 固定費 / (ARPU × gross_margin)
break_even_MRR = break_even_customers × ARPU

# 必要資金
total_burn = Σ(月次赤字)（黒字化まで）
buffer = 6ヶ月分の固定費
required_funding = total_burn + buffer
```

### アクション
1. `outputs/financial_model.yml` を更新

---

## Phase 5: 投資家資料生成

### 目的
ピッチデックとエグゼクティブサマリーの生成

### 入力
- 全worksheetsの値
- `outputs/financial_model.yml`

### 生成物

#### outputs/pitch_deck.md
```markdown
# {会社名}

{タグライン}

---

## 問題
{problem.statement}

## 解決策
{solution.statement}

## 市場規模
- TAM: {tam}
- SAM: {sam}
- SOM: {som}

## ビジネスモデル
- 収益モデル: {revenue_model}
- ARPU: {arpu}
- LTV/CAC: {ltv_cac_ratio}

...
```

#### outputs/executive_summary.md
```markdown
# {会社名} - エグゼクティブサマリー

## 概要
{elevator_pitch}

## 問題
{problem}

## 解決策
{solution}

...
```

---

## エラーハンドリング

### 情報不足の場合
```
→ AskUserQuestionで追加質問
```

### Web検索で情報が見つからない場合
```
→ 業界平均のデフォルト値を使用
→ 「推定値」としてマーク
→ ユーザーに確認
```

### ユニットエコノミクスが不健全な場合
```
→ 警告を表示
→ 3つの改善オプションを提示
→ ユーザーに選択させる
→ 再計算
```

---

## コマンド対応表

| ユーザーの発言 | Claude Codeのアクション |
|---------------|------------------------|
| 「スタートアップを作りたい」 | Phase 0から開始 |
| 「続きをやって」 | 最後のPhaseから再開 |
| 「今どこまで？」 | 進捗状況を表示 |
| 「ARPUを5万円にしたら？」 | シミュレーション実行 |
| 「ピッチ資料作って」 | Phase 5を実行 |
| 「ユニットエコノミクス見せて」 | 現在の指標を表示 |
| 「競合分析して」 | Phase 2の競合分析を実行 |
| 「財務モデル更新して」 | Phase 4を再実行 |

---

## ファイル更新ルール

1. **更新前に必ず現在の値を読む**
2. **計算結果は根拠とともに記入**
3. **`_meta.status` と `_meta.updated_at` を更新**
4. **完了したチェックリスト項目を `true` に**
5. **変更内容をユーザーに報告**
