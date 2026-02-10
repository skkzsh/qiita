---
title: GitHub org-mode記法 チートシート
tags:
  - Emacs
  - GitHub
  - org-mode
private: false
updated_at: '2025-12-04T19:33:05+09:00'
id: ffb63459b9cce7dd6a8c
organization_url_name: null
slide: false
ignorePublish: false
---
## はじめに
**GitHub**で使える軽量マークアップ言語としてはMarkdownだけでなく**org-mode**も使えます.

https://docs.github.com/en/repositories/working-with-files/using-files/working-with-non-code-files#rendering-differences-in-prose-documents

Emacs使いの皆さまにおかれましてはGitHubでもorg記法で書いてることでしょう.

org記法のとおり書けば, おおよそ期待どおりレンダリングされます[^1]が,
GitHub独自のMarkdown記法があるので, それに対応したGitHub独自のorg記法のチートシートを作りました (一部だけ).

[^1]: https://github.com/novoid/github-orgmode-tests

## チートシート
### 基本的な書き方とフォーマットの構文

https://docs.github.com/ja/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax


#### テキストのスタイル設定

| スタイル   | 構文  | 例                            | 出力                                |
|------------|-------|-------------------------------|-------------------------------------|
| 下付き文字 | `_{}` | `これは_{下付き}テキストです` | これは<sub>下付き</sub>テキストです |
| 上付き文字 | `^{}` | `これは^{上付き}テキストです` | これは<sup>上付き</sup>テキストです |

#### 脚注

```conf
#+OPTIONS: f:t

脚注です[fn:1].

[fn:1] 参考資料
  
中略
```

レンダリング結果:
<img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/6361/0a3150f9-dc73-c72d-2bd7-a3c1e58449a3.png" width="120px" alt="footnote">

:::note warn
Markdownのように脚注は末尾につきません
:::

https://github.com/github/markup/issues/1094


#### アラート記法

```conf
#+BEGIN_QUOTE
[!NOTE]
コンテンツをざっと読む場合でも、ユーザーが知っておくべき有益な情報
#+END_QUOTE

#+BEGIN_QUOTE
[!TIP]
物事をより良く、あるいはより簡単に行うための有益なアドバイス
#+END_QUOTE

#+BEGIN_QUOTE
[!IMPORTANT]
目標を達成するためにユーザーが知っておくべき重要な情報
#+END_QUOTE

#+BEGIN_QUOTE
[!WARNING]
問題を回避するために、ユーザーの即時の注意が必要な緊急情報
#+END_QUOTE

#+BEGIN_QUOTE
[!CAUTION]
特定の行動のリスクや否定的な結果について助言する
#+END_QUOTE
```

レンダリング結果:
<img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/6361/fac5ed92-f9cb-7806-a0a7-8ab1e9bc70a0.png" width="520px" alt="alerts">

### コードブロックの作成とハイライト

https://docs.github.com/ja/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks

```ruby
#+BEGIN_SRC ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
#+END_SRC
```

レンダリング結果:
<img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/6361/84a813a2-7e2f-969e-485c-3aeb7d13b8d5.png" width="520px" alt="code-highlight">


### ダイアグラムの作成

https://docs.github.com/ja/get-started/writing-on-github/working-with-advanced-formatting/creating-diagrams

```ruby
#+BEGIN_SRC mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
#+END_SRC
```

レンダリング結果:
<img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/6361/0362ee6b-1110-3296-37ac-584438c4d4db.png" width="320px" alt="mermaid">

### 数式の記述

https://docs.github.com/ja/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions

#### インライン式

```conf
この文は ~$~ 区切り記号を使ってインラインで数学を表している:  $\sqrt{3x-1}+(1+x)^2$
```

レンダリング結果:
<img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/6361/63335f60-1a2e-ce2b-1122-75b7c693671a.png" width="620px" alt="math-inline">

#### ブロック式

```conf
*コーシー・シュワルツの不等式*

#+BEGIN_SRC math
\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)
#+END_SRC
```

レンダリング結果:
<img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/6361/1a4bd190-91e4-0501-8e5d-cf236b7f0d27.png" width="520px" alt="math-block">

### 対応してない？ (対応して欲しい) org記法

#### 下線

```
_下線_
```

#### タスクリスト

https://docs.github.com/ja/get-started/writing-on-github/working-with-advanced-formatting/about-task-lists

```java
- [x] 下線に対応する
- [ ] 脚注に対応する
- [ ] タスクリストに対応する
```

:::note alert
`[ ]` や `[x]` がレンダリングされません
:::

レンダリング結果:
<img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/6361/48b27b4d-e686-8879-a9f1-7ef0bc5e9177.png" width="280px" alt="math-block">

## 参考
なお,

https://github.com/github/markup

によると, パーサーは以下を使ってるようです.

https://github.com/wallyqs/org-ruby


## Tips: `BEGIN_` ~ `END_` ブロックをタイプするのは手間なので

キーバインドとしては, [コメント](https://qiita.com/skkzsh/items/ffb63459b9cce7dd6a8c#comment-115bd5d193f289ac8467)でご紹介いただいた

https://orgmode.org/manual/Structure-Templates.html

で展開する, または

yasnippet などでスニペット化しておくと便利です.
以下の例では `` `m[TAB]`` や `!a[TAB]` と打てば, mermaidやアラート記法のブロックが展開されるようになります.

```conf:mermaidのスニペット例
# -*- mode: snippet -*-
# name: mermaid
# key: `m
# --
#+BEGIN_SRC mermaid
graph ${1:LR};
${2:A} --> ${3:B};
$0
#+END_SRC
```

```conf:アラート記法のスニペット例
# -*- mode: snippet -*-
# name: alert
# key: !a
# --
#+BEGIN_QUOTE
[!${1:NOTE}]
$0
#+END_QUOTE
```

https://joaotavora.github.io/yasnippet/
