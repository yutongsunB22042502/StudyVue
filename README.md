插值表达式不具有**标签解析功能**
# vue指令：
## v-html 
放在HTML标签的属性中动态解析标签
```
<div v-html=“表达式”></div>
```

## v-show/v-if

v-show通过**切换css的 display: none** 来控制元素的显示隐藏
v-if根据 判断条件 控制元素的 **创建 和 移除 （条件渲染）**

适用场景：
v-show用在需要频繁切换元素显示和隐藏的地方
v-if用在不频繁切换的场景


div里面的代码：
```
<div v-show="flag" class="class">这是一个v-show控制的盒子</div>
<div v-if="flag" class="class">这是一个v-if控制的盒子</div>
```

script里面的代码：
```
<script>
        const app = new Vue({
            el: '#app',
            data:{
                message:' <a href="https://www.baidu.com">百度</a>',
                flag:false //通过修改flag里面的真假值来控制两个盒子的显示
            }
        })
    </script>
```

控制台里面显示的元素：
```
//根据判断条件切换css的 display: none 来控制显示隐藏
<div class="class" style="display: none;">这是一个v-show控制的盒子</div>
//v-if控制的盒子未显示，根据 判断条件 控制元素的 创建 和 移除 （条件渲染）
```

## v-if、v-else/v-else-if
```
<div v-if="表达式">男</div>//表达式结果为true显示
<div v-else>女</div>//表达式为false显示
```

```
		<P v-if="score>=90" >成绩评定A</P>

        <P v-else-if="score>=70" >成绩评定B</P>

        <P v-else-if="score>=60" >成绩评定C</P>

        <P v-else >成绩评定D</P>
```

## v-on
作用：注册事件=添加监听+提供处理逻辑
语法：
**1、v-on:事件名="内联语句"**
```
        <button @click="count--">-</button>//v-on:可简写为@

        <span>{{count}}</span>

        <button v-on:click="count++">+</button>
```

**2、v-on:事件名="methods中的函数名"**
```
    <div id="app">

        <button @click="fn">显示/隐藏</button>//对应methods里面的fn()函数

        <p v-show="isshow">我是p标签</p>

    </div>
    <script>

        //一旦引入vuejs，就可以使用Vue构造函数来创建Vue实例

        const app = new Vue({
            el: '#app',

            //data属性用于指定Vue实例的数据

            data:{//响应式数据

                isshow:true,

            } ,

            methods:{//用于存放方法

                fn () {

                    this.isshow = !this.isshow//methods函数里面的this指向Vue实例

                }

            }

        })
    </script>
```

**v-on 调用传参，在函数名后面添加括号，括号后面包含需要传递的参数**
```
    <div id="app">

    <div class="box">

        <h3>muli售货机</h3>

        <button @click="fn(20)">黎深吧唧20元</button>

        <button @click="fn(10)">黎深拍立得10元</button>

        <p>余额={{money}}</p>

    </div>

    </div>
    
        <script>

        //一旦引入vuejs，就可以使用Vue构造函数来创建Vue实例

        const app = new Vue({

            //el属性用于指定Vue实例的挂载点

            el: '#app',

            //data属性用于指定Vue实例的数据

            data:{//响应式数据

                money: 100,

            } ,

            methods:{//用于存放方法

                fn (price) {

                    this.money -= price

                }

            }

        })

    </script>
```
## v-bind
作用：动态设置html的标签属性 --> src url title ...
语法：v-bind:属性名=“表达式”
注意：简写形式    :属性名="表达式"
```

        <img v-bind:src="imgurl" :title="msg" alt="hudie">//v-bind可省略，msg必须在data里面申明

  
        data:{//响应式数据

                imgurl: './image/butterfly.jpg',

                msg: '蝴蝶'

            }

```
图片切换案例：
1、设置图片数组
2、明确数组下标
3、在图片标签中动态设置路径
4、设置button中的index，减的时候切换上一张，加的时候切换下一张
5、去掉bug（在切换到第一张或最后一张时，如果还点击上一张或下一张，图片会消失，因为数组里面没有对应下标），通过v-show来debug
```
        <button v-show="index>0" @click="index--">上一张</button>

        <img :src="imgurl[index]" :title="msg" alt="">

        <button v-show="index<imgurl.length-1" @click="index++">下一张</button>


            data:{//响应式数据

                index:0,

                msg:'黎深qq人',

                imgurl:[

                    './image/1.jpg',

                    './image/2.jpg',

                    './image/3.jpg',

                    './image/4.jpg',

                    './image/5.jpg',

                ],

  

            }
```

***
## v-for
作用：基于数据循环，多次渲染整个元素 —>数组、对象、数字...
遍历数组语法：
v-for="(item,index) in 数组"，item为数组中的每一项，index为数组下标
```
        <h3>muli水果店</h3>

    <ul>

        <li v-for="(item,index) in list" >//in与)之间有空格！

        {{item}} - {{index}}

        </li>

    </ul>

    <ul>

        <li v-for="item in list" >//index可省略

        {{item}}

        </li>
```
图书管理案例
基本需求：列出书籍作者及删除按钮，点击删除按钮可删除整行
解决思路：书籍作者可放在一个数组中，里面添加id进行唯一标识。
利用v-for将数组中的信息展现出来，删除 按钮标签中添加v-on点击事件和方法函数
方法函数中通过filter把需要删除的id序列移除，并返回新数组给原数组进行更新
具体代码：
```
		<ul>

            <li v-for="(item,index) in booklist" :key="item.id">

                <span>{{item.name}}</span>

                <span>{{item.author}}</span>

                <!--注册点击事件-> 通过id进行删除数组中的对应项 -->

                <button @click="del(item.id)" >删除</button>

            </li>

        </ul>


            data:{//响应式数据

                booklist:[

                    { id:1,name:'《红楼梦》',author:'曹雪芹'},

                    { id:2,name:'《水浒传》',author:'施耐庵'},

                    { id:3,name:'《西游记》',author:'吴承恩'},

                    { id:4,name:'《三国演义》',author:'罗贯中'},

                ]

            } ,

            methods:{

                del(id){

                    this.booklist = this.booklist.filter(item => item.id !== id)

                }

            }
```
