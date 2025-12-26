# AI Combinator

**Claude Codeが自動実行してMRR 400万円のビジネスを構築するワークブック**

## 使い方

```
あなた: 「スタートアップを作りたい」
Claude Code: 質問 → 調査 → 計算 → ファイル更新 → 資料生成
```

これだけ。Claude Codeが全て自動で実行します。

## Claude Codeが行うこと

| Phase | Claude Codeの動作 |
|-------|------------------|
| 0 | 質問して基本情報を収集 |
| 1 | Web検索で市場調査、ユニットエコノミクス計算 |
| 2 | 競合分析、TAM/SAM/SOM算出 |
| 3 | MVP設計、技術選定 |
| 4 | 24ヶ月財務モデル生成 |
| 5 | 投資家向けピッチデック生成 |

## ファイル構成

```
ai-combinator/
├── CLAUDE_EXECUTION.md               # ★Claude Codeへの実行指示（MD）
├── README.md
├── config.yml                        # 基本設定（自動更新）
│
├── worksheets/                       # データ・計算用（YML）
│   ├── 00_unit_economics.yml         # ユニットエコノミクス
│   ├── 01_idea.yml                   # 問題・ソリューション
│   ├── 02_market.yml                 # 市場・競合
│   ├── 03_product.yml                # MVP
│   ├── 04_business_model.yml         # ビジネスモデル
│   ├── 05_growth.yml                 # 成長戦略
│   └── 06_metrics_tracking.yml       # メトリクス追跡
│
├── outputs/                          # 生成される成果物
│   ├── pitch_deck.md                 # ピッチデック（MD）
│   ├── executive_summary.md          # エグゼクティブサマリー（MD）
│   └── financial_model.yml           # 財務モデル（YML・計算用）
│
└── startup_guide.yml                 # 参照用知識ベース
```

**形式の使い分け:**
- **MD** → 人間が読むもの（指示書、資料）
- **YML** → データ・計算用（ワークシート、財務モデル）

## 核心原則：ユニットエコノミクス

Claude Codeは以下の健全性を常にチェックします：

| 指標 | 目標値 | 不健全な場合 |
|------|--------|-------------|
| LTV/CAC | > 3 | 価格改定 or CAC削減を提案 |
| CAC回収期間 | < 12ヶ月 | チャネル見直しを提案 |
| 粗利率 | > 70% | コスト構造見直しを提案 |
| 月次チャーン | < 5% | リテンション施策を提案 |

## 実行例

### 1. 開始
```
あなた: SaaSで中小企業の経理業務を自動化したい
```

### 2. Claude Codeの質問
```
Claude Code: いくつか質問させてください

1. ターゲット顧客は？
   - B2B - 大企業
   - B2B - 中小企業（10-100人） ←
   - B2B - スタートアップ

2. 想定価格帯は？
   - 月額1万円以下
   - 月額1-5万円 ←
   - 月額5-20万円

3. 顧客獲得方法は？
   - SEO/コンテンツ ←
   - 有料広告
   - 営業
```

### 3. Claude Codeの自動処理
```
Claude Code:
✓ Web検索: 経理SaaS市場規模、競合価格、業界CAC
✓ 計算: ARPU 3万円、LTV 48万円、CAC 10万円
✓ 検証: LTV/CAC = 4.8 ✓ 健全
✓ 更新: worksheets/00_unit_economics.yml
✓ 計算: MRR400万に必要な顧客数 = 134社
```

### 4. 成果物生成
```
Claude Code:
✓ outputs/pitch_deck.yml 生成完了
✓ outputs/financial_model.yml 生成完了

投資家向け資料が完成しました：
- 10スライドのピッチデック
- 24ヶ月の財務予測
- ユニットエコノミクス分析
```

## コマンド例

| コマンド | 動作 |
|---------|------|
| `スタートアップを作りたい` | Phase 0から開始 |
| `続きをやって` | 中断したところから再開 |
| `今どこまで進んでる？` | 進捗確認 |
| `ARPUを5万円にしたら？` | シミュレーション実行 |
| `ピッチ資料を作って` | pitch_deck.yml生成 |
| `ユニットエコノミクスを見せて` | 現在の指標を表示 |

## MRR 400万円の計算ロジック

Claude Codeは自動で以下を計算します：

```
入力: ARPU、チャーン率、CAC、成長率
  ↓
計算:
  - LTV = ARPU × 粗利 × (1/チャーン)
  - LTV/CAC比率
  - 必要顧客数 = 400万 / ARPU
  - 必要月数 = log(目標/現在) / log(1+成長率)
  - 必要資金 = 累積CAC + 固定費 × 月数
  ↓
出力: 財務モデル、必要資金、達成時期
```

## 健全性チェックの例

```yaml
# Claude Codeが自動で警告
警告: LTV/CAC = 2.1（目標: 3以上）

原因分析:
- CAC 15万円が高すぎる
- 有料広告のCACが25万円で平均を押し上げ

改善提案:
1. オーガニック比率を60%→80%に（CAC 10万円に改善）
2. ARPU 3万→4万円に値上げ（LTV 64万円に改善）
3. チャーン5%→3%に改善（LTV 80万円に改善）

どれを試しますか？
```

---

## 参考文献

### ユニットエコノミクス
- [A Quick Guide to Unit Economics (a16z)](https://a16z.com/a-quick-guide-to-unit-economics/)
- [SaaS Metrics 2.0](https://www.forentrepreneurs.com/saas-metrics-2/)

### Y Combinator
- [YC's Essential Startup Advice](https://www.ycombinator.com/library/4D-yc-s-essential-startup-advice)
- [Startup School](https://www.startupschool.org/)

### SaaSベンチマーク
- [OpenView SaaS Benchmarks](https://openviewpartners.com/saas-benchmarks/)
- [KeyBanc SaaS Survey](https://www.key.com/kco/images/2023_KBCM_SaaS_Survey.pdf)
