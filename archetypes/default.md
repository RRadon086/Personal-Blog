---
date: '{{ .Date }}'
draft: false
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
summary: '文章简介'
cover:
	image: 文章封面图。也支持HTTPS
categories: '文章所处的分类'
---
