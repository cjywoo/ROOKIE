# 布局技巧

## 图片实现宽高比
```
width:100%
height:0
padding-bottom:百分比%
```
因为这样写，padding是相对于宽度的。而如果height写百分比，是相对于父亲元素的

## flex布局，文字超出之后...
flex布局且设置flex: 1 之后，需要设置min-width才能实现文件超出之后....

```
.item-info
        flex: 1
        min-width: 0
        padding: .1rem
        .item-title
            line-height: .54rem
            font-size: .32rem
            ellipsis()
```
