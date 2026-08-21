---
name: fix-article
description: leadus科目リポジトリ(math・phys等)でGitHub issueとして起票済みの記事改善issueに対応し、新規ブランチを作成して記事を修正し、pushしてPRを作成する際に使う。「issueに従って直して」「このissueを直してPRまで出して」「さっきのissue直して」「修正してPR作って」といった依頼で使用する。issueの指定がない場合は、同じ会話内で直前に[[create-issue]]スキルが起票したissueをデフォルトの対象として使う。それがない場合はユーザーにリポジトリとissue番号(またはURL)を確認する。SKIP: 対応すべきGitHub issueが存在せず単に記事を編集してほしいだけの依頼(通常の編集で対応)、PR作成後のレビュー・マージ自体を頼まれた場合(これはメンテナーの手動レビュー工程でありスキル対象外)、issueの起票自体を頼まれた場合([[create-issue]]の担当)。
---

# issueに基づく記事修正PRの作成

## 前提

- 対象リポジトリ(`math`・`phys`等)はいずれも `leadus-info` organization 配下で、ユーザーはADMIN権限を持つ。フォーク不要で `origin` に直接ブランチをpushしてPRを作成してよい。
- 各科目リポジトリの規約は `WORKFLOW.md`・`CONTRIBUTING.md`・`GUID.md`(該当科目)を参照する。手順・命名規則が本ファイルの記載と食い違う場合は現物のドキュメントを優先する。ただし**ブランチ命名(手順4)は本ファイルの規則を優先する**。
- 記事の修正・レビューは[[edit-article]]スキルのFlow B(検証付き修正)に委ねる(手順5)。修正内容(issueの内容)は既に具体化しているため、[[edit-article]]側で取捨選択は行われず、修正→Student/Professorによる検証を1回行った時点で打ち切られる(無限にレビュー・修正を繰り返さない)。

## 手順

1. **対象issueの特定**
   - 直前の会話で[[create-issue]]スキルがissueを起票していれば、そのrepo・issue番号をそのまま使う。
   - なければユーザーにリポジトリ名とissue番号/URLを尋ねる。番号が分からない場合は `gh issue list --repo leadus-info/<repo> --state open --label improvement` で候補を提示して選んでもらう。

2. **issue内容の取得**
   ```
   gh issue view <番号> --repo leadus-info/<repo> --json title,body,url,labels
   ```
   bodyから「対象記事」「現状の問題点」「改善案」を読み取る。

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

5. **[[edit-article]]スキルによる修正とレビュー(Flow B: 検証付き修正)**
   - issueの「対象記事」「現状の問題点」「改善案」を入力として[[edit-article]]スキルのFlow Bを実行する。
   - Flow B内でEditorが`CONTRIBUTING.md`(フロントマター、H2から開始、`requires`整合性、Tips/Note使い分け、中立性、対象`stage`のチェックリスト)・`GUID.md`(科目固有の注意点)に従って記事を修正し、Student・Professorによる検証を1回行う。
   - 検証で指摘が残った場合でも、[[edit-article]]の方針通りそこで打ち切られる(自動で修正ラウンドを繰り返さない)。残った指摘があれば次の手順でユーザーに提示し、コミット前に対応するかどうかの判断を仰ぐ。

6. **コミット**
   - 変更内容が分かる簡潔なコミットメッセージを付ける。issue番号への言及はコミットではなくPR側で行う。

7. **push・PR作成前の確認**
   - originへのpushとPR作成はリポジトリ外に見える行為のため、diffの要約(および手順5で残った指摘があればその内容)を提示し、実行してよいかユーザーに確認する。

8. **push・PR作成**
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
   - edit-articleスキル(Flow B)でレビュー・修正したことを確認した(検証後に残った指摘があればその旨も記載する)
   EOF
   )"
   ```
   - `WORKFLOW.md` §5 の方針(1PR=1トピック・1難易度、`Closes #番号`で紐づけ)に従う。

9. **結果の報告**
    - 作成されたPRのURLをユーザーに伝える。
