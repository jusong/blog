---
layout: post
title:  "emacs使用markdown-mode编辑及预览markdown"
date:   2017-01-07 18:27:00 +0800
author: blacknc
categories: svn
---

**平时写代码用惯了emacs，所以在写markdown的时候也想用emacs编辑，主要是习惯了emacs纯键盘控制编辑的方式，不习惯鼠标点来点去的**


### 1. 首先给emacs开启makrdown-mode功能


#### a.下载markdown-model到load-path目录里：

`$ wget http://jblevins.org/projects/markdown-mode/markdown-mode.el -P ~/.emacs.d/lisp`

其中~/.emacs.d/lisp是我设置的load-path目录，设置方式是在~/.emacs文件里添加如下内容
`(add-to-list 'load-path "~/.emacs.d/lisp")`


#### b.修改~/.emacs文件，开启markdown功能

```
cat >> ~/.emacs <<EOF
;;Markdown Mode
(autoload 'markdown-mode "markdown-mode"
   "Major mode for editing Markdown files" t)
(add-to-list 'auto-mode-alist '("\\.markdown\\'" . markdown-mode))
(add-to-list 'auto-mode-alist '("\\.md\\'" . markdown-mode))
EOF
```

此时使用emacs编辑以.makrdown或者.md为扩展名的文件会自动开启makrdown-mode模式，提供了关键字高亮，快捷键绑定的语法模板等



### 2. markdown的预览

**要使用makrdown-moded的预览功能需要安装markdown命令，具体安装方法可自行百度/Google**


makrdown-mode提供了几种预览方式


`C-c C-c m`		;;在另一个buffer空间里显示对当前编辑的文件运行makrdown命令输出的结果，即将当前文件转换成html源代码


`C-c C-c p`		;;在浏览器里打开预览，其实就是临时生成一个html文件并用浏览器进行展示


`C-c C-c e`		;;默认在当前文件所在目录生成一个同名的html文件


`C-c C-c v`		;;默认在当前文件所在目录生成一个同名的html文件，并用浏览器打开该html文件进行预览


`C-c C-c l`		;;默认在当前文件所在目录生成一个同名的html文件，并在另一个buffer空间里预览最终显示的效果，我比较喜欢这种方式，不过emacs默认是纵向打开新buffer，可以将其调整成横向打开

```
cat >> ~/.emacs <<EOF
;;修改启动两个窗口时的默认布局方式为横向布局
(setq split-height-threshold nil)
(setq split-width-threshold 0)
EOF
```



另外如果不想在当前文件所在目录生成html文件的话（比如在git仓库下编辑README.md），可以修改makrdown-mode.el文件中的html文件路径为/tmp/下面，将其中的如下内容

```
(defun markdown-export-file-name (&optional extension)
  "Attempt to generate a filename for Markdown output.
The file extension will be EXTENSION if given, or .html by default.
If the current buffer is visiting a file, we construct a new
output filename based on that filename.  Otherwise, return nil."
  (when (buffer-file-name)
    (unless extension
      (setq extension ".html"))
    (let ((candidate
           (concat
            (cond
             ((buffer-file-name)
              (file-name-sans-extension (buffer-file-name)))
             (t (buffer-name)))
            extension)))
      (cond
       ((equal candidate (buffer-file-name))
        (concat candidate extension))
       (t
        candidate)))))
```

修改为

```
(defun markdown-export-file-name (&optional extension)
  "Attempt to generate a filename for Markdown output.
The file extension will be EXTENSION if given, or .html by default.
If the current buffer is visiting a file, we construct a new
output filename based on that filename.  Otherwise, return nil."
  (when (buffer-file-name)
    (unless extension
      (setq extension ".html"))
    (setq tmpdir "/tmp/")
    (concat tmpdir (buffer-name) extension)))
```

或者也可以使用我打的补丁进行修改，**注意将如下命令中的*~/.emacs.d/lisp*目录修改为markdown-mode.el文件的实际所在目录**

```
cd ~/.emacs.d/lisp
wget -q -O - http://7xnc41.com1.z0.glb.clouddn.com/markdown-mode.path | patch -p1
```

至此就可以用emacs正常编辑预览markdown文件了，不过不是实时预览，需要保存后才会自动刷新预览
