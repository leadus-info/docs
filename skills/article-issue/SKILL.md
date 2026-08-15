---
name: article-issue
description: leadus科目リポジトリ(math・phys等、`<subject>/{beginner,intermediate,advanced,bachelor,master}/`配下)の特定記事について、既に洗い出されている問題点をGitHub issueとして起票する際に使う。「この指摘をissueにして」「問題点をissueに起票して」「このレビュー結果でissue立てて」「さっきの指摘まとめてissue化して」といった依頼で使用する。問題点そのものはユーザーが直接列挙する場合もあれば、article-critique等の別スキルの出力を渡される場合もある——本スキルはそれらを入力として受け取るだけで、問題点の洗い出し自体は行わない。起票には対象リポジトリの`.github/ISSUE_TEMPLATE/article-improvement.yml`(「既存記事の改善提案」テンプレート)を用いる。SKIP: issueを立てずに直接記事を修正してほしい依頼(誤字・表記ゆれ等の軽微な修正はWORKFLOW.mdの方針でissue不要、直接編集で対応)、対象が科目記事のパスではない場合(platform/assets-web等のインフラ・基盤issueは対象外)、issueへの起票ではなくPRの作成・修正そのものを頼まれた場合([[article-fix-pr]]の担当)。
---

# 記事issueの起票

## 手順

1. **対象記事とリポジトリの特定**
   - ユーザーが指定した記事パス(例: `math/advanced/analysis/derivatives.md` や `advanced/analysis/derivatives.md`)から、先頭の科目名(`math`・`phys`等)を repo として特定する。
   - 曖昧な場合は記事パスをユーザーに確認する。
   - 対象は必ず `~/work/<repo>/` 配下の記事であること。`~/work/CLAUDE.md` の方針どおり、操作は該当サブリポジトリ内(`~/work/<repo>/`)で行う。

2. **軽微な修正のみでないか確認**
   - 入力された問題点が誤字・表記ゆれなど軽微なものだけの場合、`WORKFLOW.md`の方針上issueを立てずに直接PRを出すべきである旨をユーザーに伝え、それでも起票するかを確認する。

3. **issue内容の組み立て**
   対象repoの `.github/ISSUE_TEMPLATE/article-improvement.yml` のフィールドに沿って構成する(mathとphysで同一構成であることを確認済み。他科目リポジトリでも同様の構成を前提とするが、初回は念のため該当ファイルを確認する)。
   - **title**: `[改善] <記事タイトルや論点の要約>`
   - **対象記事**: `<repo>/<stageを含む相対パス>` の形式(テンプレートの例: `math/advanced/analysis/derivatives.md`)
   - **現状の問題点**: 入力された問題点を箇条書きで整理する。該当箇所の引用があれば活かす。指摘の出典(誰が指摘したか、article-critique等)は本文に含めず、問題点そのものに絞る。
   - **改善案**: 入力に改善の方向性が含まれていれば記載する。なければ空欄でよい(テンプレート上必須ではない)。
   - **labels**: `improvement`

4. **ユーザーへの確認**
   - issueの起票はリポジトリ上で他者にも見える行為なので、実行前にtitle・bodyのドラフトを提示し、実行してよいか確認する。

5. **起票の実行**
   ```
   gh issue create --repo leadus-info/<repo> --title "<title>" --label improvement --body "<body>"
   ```
   - `<repo>` は `math`・`phys` 等、対象科目リポジトリ名。
   - bodyはテンプレートの見出し(`### 対象記事` `### 現状の問題点` `### 改善案`)を保った形式で組み立てる。

6. **結果の報告**
   - 作成されたissueのURLと番号をユーザーに伝える。
   - 続けて修正・PR作成まで行いたい場合は[[article-fix-pr]]スキルで引き継げる旨を伝える。
