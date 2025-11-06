# Shift Solver — シフト自動作成システム（公開版）

## 概要

胚培養室におけるシフト作成を自動化するために開発した、  
Python と Google OR-Tools を用いた勤務表最適化システムです。

スタッフの勤務希望・休日数・連勤制限・グループ別配置など多数の制約を同時に満たしつつ、  
公平性と柔軟性を両立したスケジュールを自動生成します。

---

## 技術構成

| 分類 | 内容 |
|------|------|
| 言語 | Python 3.x |
| 使用ライブラリ | OR-Tools (CP-SAT Solver), pandas, openpyxl |
| 入出力形式 | Excel（勤務希望表を入力し、結果を自動出力） |
| 実行環境 | JupyterLab または コマンドライン |

---

## 主な特徴

- 勤務希望・休暇・制約を反映  
- 4連勤制限、公休数、シフト人数など複数条件を同時に最適化  
- 早番・遅番の回数を均等化し、公平性をスコア化  
- 複数の最適解を生成する（5パターン）  
- Excelファイルを直接入出力し、現場業務へ即導入可能  

---

## 制約条件の例

```python
# Hard constraint: No more than 4 consecutive workdays
for i in range(num_staff):
    for d in range(num_days - 4):
        m.Add(sum(x[(i, d+k, WORK)] for k in range(5)) <= 4)

# Hard constraint: At least one A group in late shift on Thursday/Sunday
for d in range(num_days):
    if is_ts_day(d):  # Thursday or Sunday
        m.Add(sum(x[(i, d, LATE)] for i in range(num_staff)
                  if staff_group[i] == "A") >= 1)
