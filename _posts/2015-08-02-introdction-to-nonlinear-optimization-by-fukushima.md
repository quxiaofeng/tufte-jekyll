---
layout: post
title:  "《非线性最优化基础》学习笔记"
date:   2015-08-02 14:33:54
categories:
---

《[非线性最优化基础](http://book.douban.com/subject/6510671/)》 作者 [福嶋雅夫](http://www.seto.nanzan-u.ac.jp/~fuku/index.html) {% sidenote 1 '《非线性最优化基础》（豆瓣链接：[http://book.douban.com/subject/6510671/](http://book.douban.com/subject/6510671/)）。福嶋雅夫（Masao Fukushima），教授，日本南山大学理工学院系统与数学科学系，日本京都大学名誉教授，加拿大滑铁卢大学/比利时那慕尔大学/澳大利亚新南威尔士大学客座教授。主页：[http://www.seto.nanzan-u.ac.jp/~fuku/index.html](http://www.seto.nanzan-u.ac.jp/~fuku/index.html)。' %}

该文为[冯象初教授](http://web.xidian.edu.cn/xcfeng/){% sidenote 2 '冯象初，教授，西安电子科技大学数学系。主页：[http://web.xidian.edu.cn/xcfeng/](http://web.xidian.edu.cn/xcfeng/)' %}有关非线性最优化的讲座的笔记。

**主要内容**

**理论基础**

1. 凸函数、闭函数
2. 共轭函数
3. 鞍点问题
4. Lagrange 对偶问题
5. Lagrange 对偶性的推广
6. Fenchel 对偶性

**算法**

1. Proximal gradient methods
2. Dual proximal gradient methods
3. Fast proximal gradient methods
4. Fast dual proximal gradient methods

<!--more-->

## 理论基础 ##

### 凸函数、闭函数 ###

给定函数 {%m%}f : \Re^n \to [-\infty, +\infty] {%em%}，称 {%m%}\Re^{n+1}{%em%} 的子集
{% math %} graph \; f = \left\lbrace ({\bf x}, \beta)^T \in \Re^{n+1} \mid \beta = f({\bf x}) \right\rbrace, {% endmath %}
为 {%m%}f{%em%} 的**图像**（graph），而称位于 {%m%}f{%em%} 的图像上方的点的全体构成的集合
{% math %}epi \; f =\left\lbrace ({\bf x}, \beta)^T \in \Re^{n+1} \mid \beta \ge f({\bf x}) \right\rbrace{% endmath %}
为 {%m%}f{%em%} 的**上图**（epigraph）。若上图 {%m%}epi \; f{%em%} 为凸集，则称 {%m%}f{%em%} 为**凸函数**(convex function)。

**定理 2.27** 设 {%m%} \mathcal{I} {%em%} 为任意非空指标集，而 {%m%}f_i : \Re^n \to [-\infty, +\infty] \; (i \in \mathcal{I}){%em%} 均为凸函数，则由
{% math %} f({\bf x}) = \sup \left\lbrace f_i({\bf x}) \mid i \in \mathcal{I} \right\rbrace{% endmath %}
定义的函数 {%m%}f : \Re^n \to [-\infty, +\infty] {%em%} 为凸函数。进一步，若 {%m%}\mathcal{I}{%em%} 为有限指标集，每个 {%m%}f_i{%em%} 均为正常的凸函数，并且 {%m%}\cap_{i \in \mathcal{I}} \; dom \; f_i \neq \varnothing {%em%}，则 {%m%}f{%em%} 为正常凸函数。

若对任意收敛于{%m%}{\bf x}{%em%}的点列{%m%}\lbrace{\bf x}^k\rbrace \subseteq \Re^n{%em%}均有
{% math %} f({\bf x}) \ge \limsup_{k \to \infty}f({\bf x}^k) {% endmath %}
成立，则称函数{%m%}f:\Re^n\to[-\infty,+\infty]{%em%}在{%m%}{\bf x}{%em%}处**上半连续**（upper semicontinuous）；反之，当
{% math %} f({\bf x}) \le \liminf_{k \to \infty}f({\bf x}^k) {% endmath %}
成立时，称{%m%}f{%em%}在{%m%}{\bf x}{%em%}处**下半连续**（lower semicontinuous）。若{%m%}f{%em%}在{%m%}{\bf x}{%em%}处既为上半连续又为下半连续，则称{%m%}f{%em%}在{%m%}{\bf x}{%em%}处**连续**（continuous）。

### 共轭函数 ###

给定正常凸函数{%m%}f:\Re^n \to (-\infty,+\infty]{%em%}，由
{% math %}f^\ast({\bf\xi}) = \sup \left\lbrace <{\bf x},{\bf\xi}>-f({\bf x}) \mid {\bf x}\in \Re^n \right\rbrace {% endmath %}
定义的函数{%m%}f^\ast:\Re^n \to [-\infty,+\infty]{%em%}称为{%m%}f{%em%}的**共轭函数**（conjuagate function）。

**定理 2.36** 正常凸函数{%m%}f:\Re^n \to (-\infty,+\infty]{%em%}的共轭函数{%m%}f^\ast{%em%}必为闭正常凸函数。

### 鞍点问题 ###

设{%m%}Y{%em%}与{%m%}Z{%em%}分别为{%m%}\Re^n{%em%}与{%m%}\Re^m{%em%}的非空子集，给定以{%m%}Y\times Z{%em%}为定义域的函数{%m%}K:Y\times Z\to[-\infty,+\infty]{%em%}，定义两个函数{%m%}\eta:Y\to[-\infty,+\infty]{%em%}与{%m%}\zeta:Z\to[-\infty,+\infty]{%em%}如下：
{% math %}\eta({\bf y})=\sup\left\lbrace K({\bf y},{\bf z}) \mid {\bf z} \in Z\right\rbrace {% endmath %}
{% math %}\zeta({\bf z})=\inf\left\lbrace K({\bf y},{\bf z}) \mid {\bf y} \in Y\right\rbrace {% endmath %}
{% math %}\min \; \; \eta({\bf y})\\s.t. \; \; \; {\bf y} \in Y{% endmath %}
{% math %}\max \; \; \zeta({\bf z})\\s.t. \; \; \; {\bf z} \in Z{% endmath %}

**引理 4.1** 对任意{%m%}{\bf y}\in Y{%em%}与{%m%}{\bf z}\in Z{%em%}均有{%m%}\zeta({\bf z}) \leq \eta({\bf y}){%em%}成立。进一步，还有
{% math %}\sup\left\lbrace \zeta({\bf z})\mid {\bf z}\in Z\right\rbrace \leq\inf\left\lbrace \eta({\bf y})\mid {\bf y} \in Y\right\rbrace {% endmath %}

**定理 4.1** 点{%m%}(\bar{\bf y},\bar{\bf z})\in Y\times Z{%em%}为函数{%m%}K:Y\times Z\to[-\infty,+\infty]{%em%}的鞍点的充要条件是{%m%}\bar{\bf y}\in Y{%em%}与{%m%}\bar{\bf z}\in Z{%em%}满足
{% math %}\eta(\bar{\bf y})=\inf\left\lbrace \eta({\bf y})\mid {\bf y}\in Y\right\rbrace =\sup\left\lbrace \zeta({\bf z})\mid {\bf z}\in Z\right\rbrace =\zeta(\bar{\bf z}){% endmath %}

{%m%}{%em%}

{% math %} {% endmath %}