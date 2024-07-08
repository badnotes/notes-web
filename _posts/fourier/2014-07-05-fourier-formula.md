---
layout: mypost
title: "傅立叶公式"
tagline: "傅立叶公式"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。傅立叶公式。"
category: Fourier
tags: [Fourier]
---



$$
X_k = \frac{1}{N} \sum_{n=1}^{N-1} X_n e^{2i{\pi}k\frac{n}{N}}
$$

这段文字能够很好的解释这个公式：

To find the energy at a particular frequency, spin your signal around a circle at that frequency, and average a bunch of points along that path.

现在对这个傅立叶还不够理解，所以很难用中文表达：

将你的信号绕着频率旋转一周，平均分布在边缘上的点，便是该频率的能量。