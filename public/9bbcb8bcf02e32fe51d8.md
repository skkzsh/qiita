---
title: orgファイルを更新すると出力ファイルも自動更新されるようにする
tags:
  - Emacs
  - org-mode
  - emacs-lisp
private: false
updated_at: '2026-01-29T00:55:21+09:00'
id: 9bbcb8bcf02e32fe51d8
organization_url_name: null
slide: false
ignorePublish: false
---
## はじめに

https://orgmode.org/ja/

Emacsのorg-modeで文章を書くと, `C-c C-e`([`org-export-dipatch`](https://orgmode.org/manual/The-Export-Dispatcher.html)) で,
[様々なファイル形式 (Markdown, HTML, PDF, LaTeXなど) に出力できます](https://orgmode.org/manual/Exporting.html).
しかし, orgファイルを更新するたびに, 手動で出力を更新するのは面倒です.

## 設定
そこで以下のように[`:hook`](https://www.gnu.org/software/emacs/manual/html_node/use-package/Hooks.html)を追加すると, orgファイル保存時に自動で出力ファイルも更新されるようにできます.
ただし, すでに出力ファイルが存在する場合にのみ自動出力するようにしています.
(例として, HTMLやMarkdownの設定のみ書いてますが, 必要に応じて他ファイル形式も追加したり, 削除したりしてください.)

```emacs-lisp:~/.emacs.d/init.el
(use-package org
  :hook
  (org-mode . (lambda ()
                ;; exportしたfileが存在すれば, 保存後にexport
                (add-hook 'after-save-hook
                          (lambda ()
                            (when (file-exists-p (concat (file-name-sans-extension (buffer-file-name)) ".html"))
                              (org-html-export-to-html)
                              )
                            (when (file-exists-p (concat (file-name-sans-extension (buffer-file-name)) ".md"))
                              (org-md-export-to-markdown)
                              )
                            )
                          nil t))
            )

  :custom
  (org-export-in-background t) ; exportをbackgroundで
  )
```

この設定により, 1度HTMLやMarkdownファイルを手動で出力すると, 以降はorgファイル更新時に自動でHTMLやMarkdownファイルも更新されるようになります.

## バージョン情報
```emacs-lisp:*scratch*
org-version
"9.8-pre"

emacs-version
"30.2"
```

## 関連記事
https://qiita.com/skkzsh/items/6328d2ec36c003be3713
