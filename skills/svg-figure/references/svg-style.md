# SVG図版スタイルガイド

leadus科目リポジトリに挿入する図版の技術的な基準値とテンプレート。「教科書の図版」を基準に、装飾を排し、サイト側のテーマ(ライト/ダーク)にも追従する作りにする。

## 基本方針

- **色はテーマに委ねる**。塗り・線とも`currentColor`を使い、独自の色コード(`#333`等)をベタ書きしない。サイトのCSS側で文字色を切り替えれば図も追従する。
- **色相で区別しない**。複数の量・要素を区別したい場合は、色を増やすのではなく次の手段を優先する。
  - 線種(実線/破線/点線)
  - ラベル・記号(A, B, F₁, F₂ など)
  - 太さ・不透明度(`opacity="0.6"`など、あくまで補助的に)
- **塗りは最小限**。面積や領域を示す目的がある場合のみ、`fill="currentColor" fill-opacity="0.08"`程度の薄いハッチング相当で使う。装飾目的の塗りは使わない。
- **固定サイズを持たない**。`width`/`height`属性は付けず、`viewBox`だけで座標系を定義する。記事側の表示領域に応じて拡大縮小させる。
- **フォントを埋め込まない**。`font-family`は指定しないか`sans-serif`とし、ブラウザ・サイト側のデフォルトフォントに委ねる。

## 基準値

| 項目 | 値 |
|------|-----|
| viewBox(横長図: グラフ・自由体図など) | `0 0 400 300` |
| viewBox(正方形図: 幾何図形など) | `0 0 400 400` |
| 主要な線の`stroke-width` | `2` |
| 補助線・目盛りの`stroke-width` | `1` |
| 補助線・隠れ線の`stroke-dasharray` | `4 3` |
| 本文ラベルの`font-size` | `16` |
| 軸目盛りなど小さいラベルの`font-size` | `13` |
| 頂点・点を示す円の半径 | `3` |
| 図全体の余白(viewBox端からの最小マージン) | `20`程度 |

線・文字とも`fill="none" stroke="currentColor"`(線)または`fill="currentColor"`(文字・点)を基本とし、個別に色を指定しない。

## 矢印(ベクトル・軸)の定義

軸や力・速度などのベクトルには`<marker>`で矢頭を定義して使い回す。SVGの先頭で1回定義する。

```xml
<defs>
  <marker id="arrow" viewBox="0 0 10 10" refX="9" refY="5"
          markerWidth="6" markerHeight="6" orient="auto-start-reverse">
    <path d="M0,0 L10,5 L0,10 z" fill="currentColor"/>
  </marker>
</defs>
```

使用例(始点から終点への矢印):

```xml
<line x1="50" y1="250" x2="200" y2="250" stroke="currentColor" stroke-width="2" marker-end="url(#arrow)"/>
```

## テンプレート例

### 1. 幾何図形(三角形・内角の和)

```xml
<svg viewBox="0 0 400 320" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="三角形の3つの内角の和が180度になることを示す図">
  <polygon points="200,40 60,280 340,280" fill="none" stroke="currentColor" stroke-width="2"/>
  <circle cx="200" cy="40" r="3" fill="currentColor"/>
  <circle cx="60" cy="280" r="3" fill="currentColor"/>
  <circle cx="340" cy="280" r="3" fill="currentColor"/>
  <text x="200" y="28" font-size="16" text-anchor="middle" fill="currentColor">A</text>
  <text x="42" y="290" font-size="16" text-anchor="middle" fill="currentColor">B</text>
  <text x="358" y="290" font-size="16" text-anchor="middle" fill="currentColor">C</text>
</svg>
```

### 2. 関数グラフ(座標軸+曲線)

```xml
<svg viewBox="0 0 400 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="y=x^2のグラフ">
  <defs>
    <marker id="arrow" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M0,0 L10,5 L0,10 z" fill="currentColor"/>
    </marker>
  </defs>
  <!-- x軸・y軸 -->
  <line x1="30" y1="250" x2="380" y2="250" stroke="currentColor" stroke-width="1.5" marker-end="url(#arrow)"/>
  <line x1="200" y1="280" x2="200" y2="20" stroke="currentColor" stroke-width="1.5" marker-end="url(#arrow)"/>
  <text x="385" y="255" font-size="13" fill="currentColor">x</text>
  <text x="205" y="20" font-size="13" fill="currentColor">y</text>
  <!-- 曲線 -->
  <path d="M 120,250 Q 200,250 280,60" fill="none" stroke="currentColor" stroke-width="2"/>
</svg>
```

### 3. 物理: 自由体図(free body diagram)

```xml
<svg viewBox="0 0 400 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="斜面上の物体にはたらく重力と垂直抗力を示す自由体図">
  <defs>
    <marker id="arrow" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M0,0 L10,5 L0,10 z" fill="currentColor"/>
    </marker>
  </defs>
  <rect x="180" y="140" width="40" height="40" fill="none" stroke="currentColor" stroke-width="2"/>
  <!-- 重力 mg -->
  <line x1="200" y1="180" x2="200" y2="250" stroke="currentColor" stroke-width="2" marker-end="url(#arrow)"/>
  <text x="208" y="245" font-size="16" fill="currentColor">mg</text>
  <!-- 垂直抗力 N -->
  <line x1="200" y1="140" x2="200" y2="80" stroke="currentColor" stroke-width="2" marker-end="url(#arrow)"/>
  <text x="208" y="90" font-size="16" fill="currentColor">N</text>
</svg>
```

## alt textの書き方

- 図が伝える結論・関係を1文で簡潔に述べる(例: 「三角形の3つの内角の和が180度になることを示す図」)。
- 「〜の図」で終わるだけの空疎な説明(例: 「三角形」)は避け、その図が何を示しているかまで含める。
- `<svg>`要素自体にも`role="img" aria-label="..."`を付け、Markdown側の`![alt](path)`のalt textと同内容にする。
