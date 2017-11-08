---
slug: vanilla-emacs-daemon
draft: true
title: init.elを読み込ませず素の状態のEmacsデーモンをサーバーとして立ち上げる
date: 2017-11-08T03:19:16.796Z
tags:
  - Emacs
---
[Initial Options - GNU Emacs Manual](https://www.gnu.org/software/emacs/manual/html_node/emacs/Initial-Options.html)を参照

`-Q` オプションをつければ良い。

```server.el
(require 'server)

(progn
  (let ((my/server-host (or (getenv "SERVER_HOST") "0.0.0.0"))
        (my/server-port (or (getenv "SERVER_PORT") "1234"))
        (my/server-name (or (getenv "SERVER_NAME") "emacs_server"))
        (my/default-directory (expand-file-name (or (getenv "") "./emacs.d"))))
    (setq server-host my/server-host)
    (setq server-port my/server-port)
    (setq server-name my/server-name)
    (setq default-directory my/default-directory)
    (setq server-use-tcp t)
    (setq server-auth-dir my/default-directory)))

(defun server-ensure-safe-dir (dir) t)

(server-start)
```

```sh
emacs -Q -daemon --load=server.el
```
