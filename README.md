# claude-marketplace

個人的に開発・公開している Claude Code プラグインのマーケットプレイスです。主に実験的なプラグインや個人用ツールを公開しています。

## マーケットプレイスの追加

このマーケットプレイスを Claude Code に追加するには、以下のコマンドを実行してください：

```shell
/plugin marketplace add mashiike/claude-marketplace
```

または Git URL を使用：

```shell
/plugin marketplace add https://github.com/mashiike/claude-marketplace.git
```

## 利用可能なプラグイン

### werewolf-game

Agent Teams で人狼ゲームを実行するプラグインです。GM（ゲームマスター）として 9 人の AI プレイヤーでターン制人狼ゲームを進行します。

**インストール：**
```shell
/plugin install werewolf-game@mashiike
```

**使用方法：**
```shell
/werewolf-game
```

詳細は [plugins/werewolf-game/README.md](plugins/werewolf-game/README.md) を参照してください。

## プラグインの開発

このリポジトリにプラグインを追加したい場合は、以下の構造に従ってください：

```
plugins/
  your-plugin/
    .claude-plugin/
      plugin.json          # プラグインマニフェスト
    skills/                # スキル（オプション）
      your-skill/
        SKILL.md
    agents/                # エージェント（オプション）
      your-agent.md
    commands/              # コマンド（オプション）
      your-command.md
    README.md              # プラグインのドキュメント
```

プラグインを追加したら、`.claude-plugin/marketplace.json` に登録してください。

## ライセンス

このマーケットプレイスは MIT ライセンスの下で公開されています。各プラグインのライセンスは個別のプラグインディレクトリを参照してください。

## 関連リンク

- [Claude Code ドキュメント](https://code.claude.com/docs)
- [プラグインマーケットプレイスのドキュメント](https://code.claude.com/docs/ja/plugin-marketplaces)
- [プラグイン作成ガイド](https://code.claude.com/docs/ja/plugins)
