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
