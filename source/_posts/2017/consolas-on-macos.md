---
title: Consolas on macOS
date: 2017-11-02 23:07:24
tags:
- font
- mac
---

Install and use _consolas_ font, my favorite from Windows world on macOS.
<img class='no-box-shadow' src='consolas-font-book.png'> </img>

<!-- more -->

# TL;DR
Download _Consolas.ttf_ font file from this Github [repo](https://github.com/tsenart/sight/blob/master/fonts/Consolas.ttf).
Add to your system Font Book by opening _Consolas.ttf_ font file and clicking _Install Font_.
<img class='no-box-shadow' src='consolas-install.png'> </img>

# Why
I'm currently working on projects and primarily using C#.

When I get used to the default font in Visual Studio, it becomes very readable to me. So after I'm home and hacking on my MacBook, I really wish I could use that font on macOS, too.

I searched online and found this [blog](http://ikato.com/blog/how-to-install-consolas-font-on-mac-os-x.html). It shows a script that extracts the font files from a Microsoft's free product with [cabextract](https://www.cabextract.org.uk) tool.

{% codeblock lang:bash http://ikato.com/blog/how-to-install-consolas-font-on-mac-os-x.html mark:9 %}
# brew is a part of Mac OS X package manager called Homebrew (http://brew.sh/).
brew install cabextract
cd ~/Downloads
mkdir consolas
cd consolas
curl -O http://download.microsoft.com/download/f/5/a/f5a3df76-d856-4a61-a6bd-722f52a5be26/PowerPointViewer.exe
cabextract PowerPointViewer.exe
cabextract ppviewer.cab
open CONSOLA*.TTF
# Press Intall font. That is all.
{% endcodeblock %}

Well, I believe someone already did that and by chance posted online (Shhh, I know it is licensed). Luckily I find it in someone's public [repo](https://github.com/tsenart/sight/blob/master/fonts/Consolas.ttf) on Github, and voila! I have it on my macOS.

# How it looks like

On Visual Studio 2017 for Mac
![](consolas-vs2017-for-mac.png)

On Terminal
![](consolas-terminal.png)

On Atom
![](consolas-atom.png)
