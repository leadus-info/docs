# Leadus ドキュメント集

本リポジトリは、Leadus Projectの共通ドキュメントを管理・公開するためのリポジトリです。

## 理念
Leadus Projectの理念は[こちら](./MANIFEST.md)をご覧ください。

## 科目の一覧
Leadus Projectの科目・接頭辞と、各科目のリポジトリの一覧は[こちら](./SUBJECTS.md)をご覧ください。

## ライセンス
Leadus Projectの成果物は、全てリポジトリ単位でライセンスを定めます。\
各リポジトリの`LICENSE`をご確認ください。

本リポジトリの[ライセンス](./LICENSE)は、本リポジトリの文書(マニフェスト・貢献ガイド・執筆ガイド等)そのものに適用されます。\
各科目リポジトリの記事には、各科目リポジトリの`LICENSE`(CC BY-SA 4.0)が適用されます。

## 貢献の流れ
Leadus Projectに参加し、コンテンツの改善・アップデートにご協力いただける方は、[貢献ガイド](./contribution/CONTRIBUTING.md)をご覧ください。

## docsと科目リポジトリの役割分担
全科目に共通する文書は本リポジトリで管理し、科目リポジトリからは本リポジトリへリンクします。科目リポジトリには、科目ごとに異なるものだけを置きます。

| 管理場所 | 管理するもの |
| --- | --- |
| docs(本リポジトリ) | [マニフェスト](./MANIFEST.md)、[貢献ガイド](./contribution/CONTRIBUTING.md)(貢献の方法と手順)、[執筆ガイド](./contribution/STYLE_GUIDE.md)(全科目共通の執筆ルール)、[科目の一覧](./SUBJECTS.md)、この役割分担 |
| [.github](https://github.com/leadus-info/.github) | GitHubの既定ファイル(organizationの全リポジトリに適用されるissueテンプレート・PRテンプレート・`CONTRIBUTING.md`)と組織プロフィール。`CONTRIBUTING.md`は本リポジトリへの案内のみを書く |
| 科目リポジトリ | `LICENSE`、`GUID.md`(科目固有の執筆ルール)、記事。`README.md`は本リポジトリへの案内のみを書く。issueテンプレート・PRテンプレート・`CONTRIBUTING.md`は置かない(.githubの既定ファイルが適用される) |

- 共通の内容を科目リポジトリに複製しないでください。複製すると、改訂時に内容が食い違う原因になります。
- 科目リポジトリにissueテンプレート等を置くと、.githubの既定ファイルが適用されなくなります。科目固有の設定が必要になった場合は、先に.githubでの扱いを検討してください。
- 科目リポジトリを新設するときは、[科目の一覧](./SUBJECTS.md)にリポジトリのURLを記載してください。
