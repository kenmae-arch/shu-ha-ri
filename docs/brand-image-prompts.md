# 画像生成プロンプト集

ChatGPT（画像生成）にそのまま貼る用。英語のほうが指示の再現性が高いため英文にしている。カラーとコンセプトは `docs/brand.md` に準拠。

**共通の注意: すべて「文字を入れない」指定にしている。** 日本語も英語も、生成AIは字形を崩すため。文字入れは生成後に別途行う。

---

## 1. アイコン（案A・推奨）— 3本線コンセプト

Zennアバター／Xプロフィール画像に共通で使う。

```
A minimalist geometric logo icon, square 1:1 composition, on a deep navy background (#0F1B2E).

Centered in the frame: three horizontal lines stacked vertically, drawn in warm off-white (#F2EFE9) with clean, precise, uniform stroke weight.
- The top line is solid and unbroken, spanning the width.
- The middle line is broken into three segments with clear visible gaps.
- The bottom line is also broken, but its segments are offset — shifted vertically out of alignment and extending past the others toward the right, suggesting release and forward motion. The rightmost segment of this bottom line is amber (#C9822F).

Flat vector design. No gradients, no shadows, no 3D, no texture. Absolutely no text, no letters, no numbers, no symbols. Generous negative space around the mark. Calm, precise, editorial technology branding. Must remain legible when scaled down to 64x64 pixels.
```

## 2. アイコン（案B）— 円相コンセプト

和のニュアンスを強めたい場合の代替案。生成してAと見比べる用。

```
A minimalist logo icon, square 1:1 composition, on a deep navy background (#0F1B2E).

Centered: a single clean circle drawn in warm off-white (#F2EFE9) with uniform thin stroke, in the spirit of a zen enso but geometric and precise rather than a rough brushstroke. The circle's stroke is interrupted by one clear gap on the right side, and from that break a single straight line extends outward beyond the circle toward the upper right, rendered in amber (#C9822F).

Flat vector design. No gradients, no shadows, no 3D, no paper texture. Absolutely no text, no letters, no symbols. Generous negative space. Calm, precise, editorial technology branding. Must remain legible at 64x64 pixels.
```

## 3. Xヘッダー（1500×500）

左下にプロフィール画像が重なり、モバイルでは左右が切れるため、**左3分の1は意図的に空けている**。生成後、右端ぎりぎりに要素が寄っていないか確認すること。

```
A wide banner image, 3:1 aspect ratio, for a technology research publication.

Deep navy background (#0F1B2E) with a very subtle darker grid pattern, barely visible. Across the right two-thirds of the canvas, three long thin horizontal lines run left to right in warm off-white (#F2EFE9):
- the top line continuous and unbroken,
- the middle line broken into segments with visible gaps,
- the bottom line broken with its segments offset out of alignment, trailing off toward the right edge, its final segment in amber (#C9822F).

The left third of the canvas is almost empty dark space with no elements. Thin, precise linework. Flat vector, minimal, calm editorial tone. No gradients, no shadows, no 3D. Absolutely no text, no letters, no numbers, no logos. Wide margins at top and bottom.
```

## 4. 記事ヘッダー／OGP用（1200×630・任意）

note併用時や、記事に画像を添えたい場合。上記と同じ世界観の背景として使い、タイトル文字は後から重ねる前提。

```
A wide background image, 1.91:1 aspect ratio, for a technology article header.

Deep navy background (#0F1B2E). In the lower right area, a sparse arrangement of thin horizontal lines in warm off-white (#F2EFE9): one solid, one broken into segments, one broken and offset with a single amber (#C9822F) segment. The upper left two-thirds is empty dark space reserved for a text overlay to be added later.

Flat vector, minimal, generous negative space, calm editorial tone. No gradients, no shadows, no 3D. Absolutely no text, no letters, no numbers.
```

---

## 修正指示の出し方（生成結果が惜しいとき）

生成し直すより、差分で直したほうが早い。ChatGPTに続けて渡す例:

- 「線が太すぎます。ストロークを半分の太さにして、線の間隔をもっと広げてください」
- 「アンバーの部分が目立ちすぎます。アクセントは1箇所だけ、面積を半分にしてください」
- 「余白が足りません。マークを20%小さくして、周囲の余白を増やしてください」
- 「文字が入ってしまっています。文字・記号を完全に取り除いてください」
