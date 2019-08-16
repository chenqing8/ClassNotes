# Less资料
1. **嵌套规则**
    `&`放在前面，表示把当前元素的所有父元素有拿到
    ```less
    a{
        b{
            c{

            }
            &:hover{
                /* 最终结果a b:hover{} */
            }
        }
    }
    ```
    `&`放在后面，表示把当前元素放到顶父级元素之前
    ```less
    a{
        b{
            c&{
                /* 最终结果c a b{} */
            }
        }
    }
    ```
    `& &`表示把前面的元素都排列组合，有几个&就组合几个
    ```less
    a,p,li{
        & &{
            /* a p,a li,p a,p li,li a,li p{} */
        }
    }
    ```
2. **运算**
    less会自动推断你的计算单位，所以不必每个数值都写个单位
    ```less
    a{
        width:450px + 400;
        height:400 + 500px;
    }
    ```
    涉及优先级，使用()来区分优先级
    ```less
    a{
        width:(900-10)*2+20px;
    }
    ```
    颜色值运算，先把颜色转化为rgb,然后在转化为16进制的值,当+的值超出255，就按255来计算,结果为负数的时候就是0
    ```less
    a{
        color:#000000 + 21;/*把#000000转化为rgb（0,0,0）然后给rgb每一位都加个21 就等于rgb（21,21,12）= #151515 */
        color:#ffffff - 55;/*把#ffffff,然后给rgb每一位都-55 就等于#c8c8c8 */
    }
    ```
3. **带返回值函数**
    `*注意：如果使用less，则需要引入less-plugin-functions，普通的less编译工具无法正常编译。*`
    ```less
    .function{
        .px2rem(@px){
            return : unit(@px/@radio , rem);
        }
    }

    #header{
        width: px2rem(640);
        height: px2rem(88);
        line-height: px2rem(88);
        background-color: #33aa33;
        text-align: center;
        font-size: px2rem(48);
        color: rgba(255,255,255,1);
    }
    ```


# Scss资料
4. **scss嵌套**
    ```css
    body{
        font-size:12px;
        font-weight:800;
        border:1px soild red;
        border-radius:50%;
    }
    ```
    ```scss
    body{
        font:{
            size:12px;
            weight:800;
        };
        border:1px soild red {
            radius:50%;
        }
    }
    ```
5. **mixin scss**
    ```scss
    @mixin alert{
        color:red;
        a{
            color:green;
        }
    }
    body{
        @include alert;
    }
    ```
    ```scss
    @mixin alert($color,$fontSize){
        color:$color;
        font-size:$fontSize;
        a{
            color:darken($color,10%);/* 在现有颜色的基础上加深10% ，变暗*/
            background:lighten($color,30%);/* 在现有颜色的基础上加亮30% ，变亮*/
            border-color:saturate($color,30%);/* 在现有颜色的基础上纯度加30% */
            border-color:desaturate($color,30%);/* 在现有颜色的基础上饱和度减30% */
            border-color:opacify($color,0.2);/* 在现有颜色的基础上加0.2的透明度，越不透明*/
            border-color:transparentize($color,0.2);/* 在现有颜色的基础上减0.2的透明度，越透明*/
        }
    }
    body{
        @include alert(red,10px);
    }
    ul{
        @include alert($fontsize:20px,$color:green)/* 参数可以改变顺序 */
    }
    ```
6. **继承**
    ```scss
    .alert{
        padding:5px;
    }
    .alert-info{
        @extend .alert;/* 这个选择器继承了.alert的选择器 */
        font-size:10px;
    }
    ```
7. **@import**每次都会发起http的请求，和less相同
8. **注释**
    强制注释：/*!<!-- 加了一个叹号表示强制注释，在源码压缩后的css中会显示 -->
              *abc
              */
9. **运算**
    8/2=8/2;
    (8/2)=4;
    (8px/2)=4px;
    (8px/2px)=4;
    2px*5px=10px*px;
    8px*2=16px;
10. **List**
    length(5px 10px)=2;<!-- 列表的长度 -->
    nth(5px 10px ,2)=10px;<!-- 得到列表中的第二项 -->
    index(1px solid red,soild)=2;<!-- 得到这个值在第几项 -->
    append(5px 10px ,2px)=(5px 10px 2px);<!-- 拼接列表 -->
    join(10px 25px,1px 0px )=(10px 25px 1px 0px);<!-- 拼接列表 -->
    join(10px 25px,1px 0px ,comad)=(10px, 25px ,1px, 0px);<!-- 用逗号连接 -->

11. **map**
    $color:(light:#ffffff,dark:#000000)
    map-get($color,light)=#ffffff;<!-- 查询key的值 -->
    map-values($color)=('#ffffff','#000000')<!-- 查询所有value值 -->
    map-keys($color)=('light','dark')<!-- 查询所有key值 -->
    map-has-key($color,light)=true<!-- 是否有该key值 -->
    map-merge($color,(gray:#dddddd))=(light:#ffffff,dark:#000000,gray:#dddddd)<!-- 合并 -->
    map-remove($color,light,dark)=(gray:#dddddd)<!-- 移除哪几个key及对应的value值 -->
12. **布尔值**
    5px>2px=true;
    (5px>2px) and (2px >5px)=false;<!-- 1false为false -->
    (5px>2px) or (2px >5px)=true;<!-- 1true为true -->
    not(5px>2px)=false;<!-- 取反 -->
13. **控制语句**
    ```scss
    $use:true;
    $lists:success error warning;
    body{
        @if $use{
            border-color:#ff0;
        }@else if{
            border-color:#f00;
        }
    }
    @for $i from 1 through 4{/* 会包含最后一次的循环 */
        .col-#{$i}{
            width:100% / 4 * $i;
        }
    }/* .col-1{width:25%}.col-2{width:50%}.col-3{width:75%}.col-4{width:100%} */
    @for $i from 1 to 4{/* 不会包含最后一个值得循环 */
        .col-#{$i}{
            width:100% / 4 * $i;
        }
    }/* .col-1{width:25%}.col-2{width:50%}.col-3{width:75%}*/
    @each $list in $lists{
        .icon-#{$list}{
            background-image:url(../images/#{$list}.png)
        }
    }
    $i:6
    @while $i>0{
        .icon-#{$i}{
            width:5px*$i;
        }
        $i:$i-2;
    }

    ```






14. **function**
    ```scss
        $color:(light:#fff,dark:#000);
        @function color($key){
            @if not map-has-key($color,$key){
                @warn "在$color中没有找到#{$key}这个key"/* 这个警告会出现在命令行中 */
            }
            @return map-get($color,$key);
        }
        body{
            color:color(light)
        }
    ```



