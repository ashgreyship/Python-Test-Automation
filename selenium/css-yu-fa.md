# CSS 语法

## 选择器

一条或多条声明

```css
{color:blue,font-size:12px}
/*color 为属性，blue 为值*/
/*这里有两条声明*/
```

##  语法：

`#kw`  选择`id`为kw的元素

## 单个选择器

### id 选择器

```css
/* id 选择器*/
/*css 中 id 选择器 以 # 来定义*/
#abc {
    color: #cc0000
}
```

### class 选择器

```css
/*class 选择器*/
/*css 当中 class 选择器以 . 来定义*/
.abd {
    color: #cc6600
}
```

###  标签选择器

```css
/*标签选择器*/
/*标签选择器可以选中同类型的html标签元素*/
p {
    background: black
}
```

### 标签选择器配合class选择器

```css
/*将标签选择器配合class选择器使用*/
a.abd {
    color: #66cc66
}
```

###  分组选择器

```css
/*分组选择器*/
/*选中一组HTML元素*/
/*在css当中，分组选择器以 , 来定义*/
p, .abe {
    font-size: 20px
}
```

###  属性选择器

```css
/* 以下实例，所有具有 title 属性的我都可以选中*/
[title] {
    color: blue
}

/*为属性指定值，以下实例将会选中所有 title 属性为 n3*/
[title="n3"] {
    font-size: 10px
}

/*也可以指定标签类型*/
p[title] {
    background: violet
}

/*还可以匹配单词边界*/
p[title~="world"] {
    background: black
}
```

## 组合选择器

* 后代选择器 以空格分隔
* 子元素选择器 以大于号分隔
* 相邻兄弟选择器 以加号分隔
* 后续兄弟选择器 以小波浪线分隔

### 后代选择器

```css
/*后代选择器*/
/*后代选择器用于选取某元素的后代元素，无论层级有多深*/
/*以下实例，将 id 为 2 的元素中的所有 p 标签选中，样式改为红色前景色*/
#n2 p {
    color: #cc0000;
}
```

###  子元素选择器

```css
/*子元素选择器*/
/*子元素选择器只能选中其父元素的直接子元素*/
#n4 > p {
    font-size: 20px
}
```

### 相邻兄弟选择器

```css
  /*相邻兄弟选择器*/
        /*可选择紧接在另一元素背后的元素，且二者拥有相同的父辈元素*/
        li + li {
            font-size: 10px;
        }

        #l1 + li {
            background: aquamarine;
        }
```

### 后续兄弟选择器

```css
/*后续兄弟选择器*/
/*选中指定元素之后的所有弟弟*/
#l1 ~ li {
    color: #cc0000
}
```

## 下标

使用 `nth-child(index)`

```css
#app >div:nth-child(1)  > div:nth-child(1)
```

## 注意：

标签有多个class

```css
#app >div > div >.card.card11:
```



