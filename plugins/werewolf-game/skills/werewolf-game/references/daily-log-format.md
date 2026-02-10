# 日次ログ YAML フォーマット

GM は各日の終了時に `{workspace}/gm/days/day-{N}.yaml` を作成する。**こまめに更新すること**（コンパクション対策）。1日の終了時には必ず完成させること。

## フォーマット

```yaml
day: 0
phase: night

# --- 夜フェーズ（0日目は密談と初日お告げのみ） ---
night:
  medium_result:             # 霊能結果（夜フェーズ開始時、処刑があった場合のみ）
    medium: "霊能者のキャラ名"
    target: "処刑されたキャラ名"
    result: "人間"           # "人狼" / "人間"（狂人・妖狐は「人間」）
  wolf_chat:
    - speaker: "キャラ名A"
      message: "発言内容そのまま"
    - speaker: "キャラ名B"
      message: "発言内容そのまま"
    # ... 全発言を記録（合計10メッセージ上限）
  divination:
    seer: "占い師のキャラ名"
    target: "占い対象のキャラ名"
    result: "人間"  # or "人狼"
    # 呪殺が発生した場合
    cursed_kill: false  # true if 妖狐を占った
  guard:
    knight: "狩人のキャラ名"
    target: "護衛対象のキャラ名"
  attack:
    wolves_choice: "襲撃対象のキャラ名"
    random_fallback: false  # true なら応答なしでGMがランダム決定
    guarded: false      # true なら護衛成功
    target_is_fox: false # true なら妖狐で襲撃無効
    result: "killed"    # "killed" / "guarded" / "fox_immune"

# --- 朝フェーズ ---
morning:
  casualties:
    - name: "キャラ名"
      cause: "attack"   # "attack" / "cursed_kill"
    # 犠牲者なしの場合は空配列 []

# --- 議論フェーズ ---
discussion:
  start_from: "キャラ名"  # 起点となるプレイヤー
  speaking_order:           # 起点から番号順に巡回した結果の順序
    - "キャラ名1"
    - "キャラ名2"
    # ...
  round_1:
    - speaker: "キャラ名1"
      message: |
        発言内容をそのまま記録する。
        要約はしない。改行もそのまま保持する。
    - speaker: "キャラ名2"
      message: |
        発言内容そのまま。
    # ... 全員分
  round_2:
    - speaker: "キャラ名1"
      message: |
        2周目の発言内容そのまま。
    # ... 全員分

# --- 投票フェーズ ---
vote:
  ballots:
    - voter: "キャラ名A"
      target: "キャラ名X"
      reason: "投票理由"
    - voter: "キャラ名B"
      target: "キャラ名Y"
      reason: "投票理由"
    # ... 全員分
  tally:
    - name: "キャラ名X"
      votes: 4
    - name: "キャラ名Y"
      votes: 3
    - name: "キャラ名Z"
      votes: 2
  abstained: []   # 応答なしで棄権したプレイヤー名のリスト
  runoff: false  # 決選投票が行われた場合 true
  executed: "キャラ名X"  # 処刑者（処刑なしの場合は null）
  last_words: |
    処刑者の遺言をそのまま記録する。

# --- 日の終了時点での状態 ---
end_of_day:
  alive:
    - "キャラ名1"
    - "キャラ名2"
    # ...
  dead:
    - name: "キャラ名"
      role: "人狼"
      cause: "executed"  # "executed" / "attacked" / "cursed_kill"
      day: 1
    # ...
  shutdowns:  # この日にshutdown_requestを送信したプレイヤー
    - name: "キャラ名"
      cause: "executed"  # "executed" / "attacked" / "cursed_kill"
      shutdown_approved: true  # プレイヤーが承認したか（true/false）
      reflection_written: true  # reflection.mdが作成されたか（true/false）
    # 複数死亡の場合は複数エントリ
    # shutdown拒否やタイムアウトの場合も記録（shutdown_approved: false）
  game_over: false  # true ならゲーム終了
  winner: null      # "村人陣営" / "人狼陣営" / "妖狐陣営" / null
```

## 日ごとの注意

### 0日目 (day-0.yaml)
- night のみ（morning, discussion, vote はなし）
- wolf_chat と divination（初日お告げ）のみ記録
- guard, attack はなし

### 1日目 (day-1.yaml)
- morning の casualties は空（1日目朝は犠牲者なし）
- night から翌朝の結果は翌日のファイルに記録

### 2日目以降 (day-N.yaml)
- 前日夜のアクション結果（attack, guard の判定結果）を morning に記録
- 以降フルフォーマット
