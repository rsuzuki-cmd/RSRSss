# RSRSss

業務用リポジトリ。**非公開（Private）で運用すること。**

## 構成

| パス | 内容 |
|------|------|
| `CLAUDE.md` | Claude Code 向けの作業ルール（レポート作成ルール・機密情報の取り扱い） |
| `.claude/settings.json` | Claude Code の権限設定（秘密情報の読み取り禁止・外部送信の確認必須化） |
| `.gitignore` | 認証情報・ローカル作業ファイル・OS 由来ファイルの除外設定 |
| `.github/workflows/secret-scan.yml` | push / PR ごとに gitleaks で認証情報の混入を検査（検出時は失敗） |
| `.pre-commit-config.yaml` | ローカル PC でコミット前に同じ検査を行う設定（任意） |
| `private/`, `local/` | 社外秘資料を手元で扱うためのフォルダ（git 管理外。必要に応じて作成） |

## 機密情報の取り扱い（要約）

- 認証情報（`.env`、鍵、トークン）や顧客情報はコミットしない。
- 社外秘資料は `private/` に置く（自動的にコミット対象外になる）。
- リポジトリの内容を外部サービス（メール、Slack、Drive 共有、Web など）へ送る操作は、Claude Code 上では必ず確認を挟む設定にしている。
- 認証情報の混入は GitHub Actions（`secret-scan`）で自動検査する。ローカルでも検査したい場合は次を実行する。

  ```bash
  pip install pre-commit
  pre-commit install
  ```

詳細は [`CLAUDE.md`](./CLAUDE.md) を参照。

## GitHub 側の推奨設定チェックリスト

リポジトリ内のファイルでは変更できない項目。GitHub の **Settings** から設定する。

- [ ] **Visibility を Private に変更**（Settings → General → Danger Zone → Change repository visibility）
- [ ] **デフォルトブランチを `main` に変更**（Settings → General → Default branch）
- [ ] **`main` にブランチ保護を設定**（Settings → Rules → Rulesets：force push 禁止、削除禁止、PR 経由のマージ必須）
- [ ] **不要な機能を無効化**（Settings → General → Features：Wiki / Projects など使わないもの）
- [ ] **アカウントの 2 要素認証を有効化**（個人の Settings → Password and authentication）
