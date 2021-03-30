### 概念
#### flex容器

设置元素的 `display` 为 `flex` 或 `inline-flex`，则其将成为flex布局的容器。

#### flex项目

若元素的父容器设置了flex布局，则其将成为flex项目。

#### 轴

假定flex容器中水平和垂直方向分别有两根轴线。则默认情况下，flex容器的水平方向为**主轴**，垂直方向为**交叉轴**。

### flex容器CSS属性:

- container
    + display: flex;
    + flex-direction: row | row-reverse | column | column-reverse;
    + flex-wrap: nowrap | wrap | wrap-reverse;
    + flex-flow: <flex-direction> || <flex-wrap>;
    + justify-content: flex-start | flex-end | center | space-between | space-around;
    + align-items: stretch | flex-start | flex-end | center | baseline;
    + align-content: stretch | flex-start | flex-end | center | space-between | space-around;

### flex项目的CSS属性：

- item
    + order: <integer>;
    + flex-grow: <number>;
    + flex-shrink: <number>;
    + flex-basis: auto | <length>;
    + flex: <flex-grow> || <flex-shrink> || <flex-basis>;
        * auto (1 1 auto)
        * none (0 0 auto)
    + align-self: auto | flex-start | flex-end | center | baseline | stretch;