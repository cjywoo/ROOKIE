# 介绍console中的“$”

## $0

* $0 是当前我们选中的html节点的引用。
* $1 是我们上一次选择的节点的引用
* $2 是在那之前选择的节点的引用

## \$与\$\$

在你还没有在app中定义$变量的情况下(例如 jQuery)，$在console中是冗长的函数document.querySelector的一个别名。
但是$$ 能节省更多的时间，因为它不仅仅执行document.QuerySelectorAll并且返回的是一个节点的数组

## $_

$_变量是上次执行的结果的引用。