---
name: fix-issue
description: >-
  leadus科目リポジトリ(math・phys等)でGitHub issueとして起票済みの記事改善issueに対応し、新規ブランチを作成して記事を修正し、pushしてPRを作成する際に使う。「issueに従って直して」「このissueを直してPRまで出して」「さっきのissue直して」「修正してPR作って」といった依頼で使用する。issueの指定がなければユーザーにリポジトリとissue番号(またはURL)を確認する。SKIP: 対応すべきGitHub issueが存在せず単に記事を編集してほしいだけの依頼(通常の編集で対応)、PR作成後のレビュー・マージ自体を頼まれた場合(これはメンテナーの手動レビュー工程でありスキル対象外)、issueの起票自体を頼まれた場合([[create-issue]]の担当)。
---

# issueに基づく記事修正PRの作成

## 前提

- 対象リポジトリ(`math`・`phys`等)はいずれも `leadus-info` organization 配下で、ユーザーはADMIN権限を持つ。フォーク不要で `origin` に直接ブランチをpushしてPRを作成してよい。
- 各科目リポジトリの規約は `WORKFLOW.md`・`CONTRIBUTING.md`・`GUID.md`(該当科目)を参照する。手順・命名規則が本ファイルの記載と食い違う場合は現物のドキュメントを優先する。ただし**ブランチ命名(手順4)は本ファイルの規則を優先する**。
- 記事の修正・レビューは[[edit-article]]スキルに委ねる(手順5)。修正内容(issueの内容)は既に具体化しているため、edit-article側でPlannerが指摘の却下/採用を新たに判断することはなく、修正プランの策定→Editorによる修正・こまめなcommit→[[review-article]]による検証、という一連の流れをedit-articleが担う。検証で指摘が残った場合、edit-articleの方針(デフォルトで最大3巡、かつ対応下限impact 3)に従って自動的に再修正・再検証されるか、issue化されるか、未解決として報告される。**対応下限はedit-articleの2巡目以降の継続判定にのみ効くため、issueに`impact: 1`・`impact: 2`のラベルが付いていても、そのissueの修正自体は必ず1巡は実行される**(ラベルを理由にedit-articleの呼び出しを省略してはならない)。

## 手順

1. **対象issueの特定**
   - ユーザーからリポジトリ名とissue番号/URLを確認する。番号が分からない場合は `gh issue list --repo leadus-info/<repo> --state open --label improvement` で候補を提示して選んでもらう。

2. **issue内容の取得**
   ```
   gh issue view <番号> --repo leadus-info/<repo> --json title,body,url,labels
   ```
   bodyから「対象記事」「現状の問題点」「改善案」を読み取る。「改善案」に「過去の修正試行(解消せず)」の記載があれば([[edit-article]]がissue化ループを打ち切った際に含まれる)、それも保持しておく。

3. **作業ディレクトリの準備**
   - `~/work/<repo>/` に移動する。
   - `git status` で作業ツリーがクリーンか確認する(未コミットの変更があれば先にユーザーに確認)。
   - `git checkout main && git pull` で最新化する。

4. **ブランチ作成**
   - ブランチ名は `fix/<issue番号>-<トピックスラッグ>` とする。
     - トピックスラッグは対象記事パスから `<repo>/`・`<stage>/`・拡張子を除き、`/`を`-`に置き換えたもの。例: issue #42、`math/advanced/analysis/derivatives.md`の修正なら `fix/42-analysis-derivatives`。
     - `fix`以外の種別(`improve`等)が必要になれば、そのつど追加する。
     - ユーザーからブランチ名の指定があれば、そちらを優先する。
   - `git checkout -b <branch>`
   - このブランチ上で[[edit-article]]を呼び出すため、edit-article側の「mainブランチならユーザーに確認する」という分岐には該当しない。

5. **[[edit-article]]スキルによる修正・検証**
   - issueの「対象記事」「現状の問題点」「改善案」を修正内容として[[edit-article]]スキルを呼び出す。「改善案」に「過去の修正試行(解消せず)」が含まれる場合、edit-article側のPlannerがそれを踏まえて同じ方針を繰り返さず別のアプローチを検討する(references/planner.mdの指示に従う)。issue本文をそのまま入力に含めていれば自然と伝わるため、fix-issue側で改めて要約・加工する必要はない。
   - edit-article内でPlannerがプランを策定し、Editorが`CONTRIBUTING.md`(フロントマター、H2から開始、`requires`整合性、Tips/Note使い分け、中立性、対象`stage`のチェックリスト)・`GUID.md`(科目固有の注意点)に従って記事を修正・commitし、[[review-article]]による検証を行う。検証結果に応じてPlannerが再修正を指示するか、issue化するか、未解決として報告するかを判断する(edit-articleの方針通り、この往復はデフォルトで最大3巡、かつ2巡目以降は対応下限impact 3以上の指摘のみ追う)。ユーザーが対応下限・巡数上限を指定した場合はそれをedit-articleの呼び出しにそのまま渡す。
   - edit-articleは対象ファイルの`git add`・`git commit`までを行い、push・PR作成は行わない。edit-articleの実行結果(反映した修正・commit一覧・issue化した指摘・未解決のまま終えた指摘・対応できなかった項目)を受け取る。

6. **push・PR作成前の確認**
   - originへのpushとPR作成はリポジトリ外に見える行為のため、edit-articleが行ったcommitの一覧(または`git log`・`git diff main...HEAD`の要約)と、残っている指摘(issue化されたもの・対応できなかったもの)を提示し、実行してよいかユーザーに確認する。

7. **push・PR作成**
   ```
   git push -u origin <branch>
   gh pr create --repo leadus-info/<repo> --base main --head <branch> \
     --title "<変更内容が分かるタイトル>" \
     --body "$(cat <<'EOF'
   Closes #<issue番号>

   ## 変更内容
   - ...

   ## 確認事項
   - CONTRIBUTING.mdの<stage>チェックリストを確認した
   - edit-articleスキルでレビュー・修正したことを確認した(残った指摘・issue化した指摘があればその旨も記載する)
   EOF
   )"
   ```
   - `WORKFLOW.md` §5 の方針(1PR=1トピック・1難易度、`Closes #番号`で紐づけ)に従う。

8. **結果の報告**
   - 作成されたPRのURLをユーザーに伝える。edit-article側でissue化された関連指摘があれば、そのissue番号もあわせて伝える。

## 参考

2026-08-23、スキル群全体の見直しに伴い`fix-article`から改称した。用途自体は変わっていないが、名称が[[edit-article]]と`fix` / `edit`で紛らわしく、両者の役割分担(このスキルはissueを起点としたブランチ作成〜PR作成までのGit運用オーケストレーションを担い、記事修正そのものの主体は`edit-article`)が名前から読み取りにくいという指摘を受けた。このスキルの実態が「issueをfixする(対応してPRを出す)」ことである点を踏まえ、対象語を`article`から`issue`に変更した。あわせて、[[edit-article]]がFlow A/Bの区別を廃止しPlanner/Editorの2ロール構成に再設計され、Editorが修正の最小単位ごとにこまめなcommitまで行うようになったため、このスキル側で担っていた「6. コミット」の手順は不要になり削除した。また`create-issue`スキルの廃止(その機能は[[edit-article]]の「issue化」に統合)に伴い、「issueの指定がなければ直前にcreate-issueが起票したissueをデフォルト対象とする」という手順も削除した。

2026-08-29、Rv指摘を原則すべてissue化する運用への転換に伴い、[[create-issue]]が独立スキルとして再新設された(詳細は[[create-issue]]・[[edit-article]]の参考参照)。このスキルが呼び出す[[edit-article]]の役割(issueの内容に基づく修正・検証)自体に変更はなく、手順1〜8は変わらない。SKIPリストの「issueの起票自体を頼まれた場合」の担当先のみ、[[edit-article]]から[[create-issue]]に更新した。
