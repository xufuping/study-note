## 1.初识Vue：

    var first = new Vue({
		el:"#box",
		data:{
                arr:["xu", "zhang"],
                json:{a:"xufu", b:"fuping"}
		},
		methods:{
			add:function(ev) {
				console.log("666");
			}
		}
	});
		

相应HTML片段：
    
    <div id="box">
		<input type="button" value="按钮" @click="add($event)">
	</div>
    
## 2.事件
### 2-1.鼠标点击事件（鼠标移动事件类似）：
    
    写法1：v-on：click="add()"
    写法2：@click="add()"

### 2-2.阻止事件冒泡：

    写法1：传参数，然后在方法里定义源生代码：ev.cancelBubble=true;
    写法2[推荐]：@click.stop="add()"

### 2-3.阻止默认事件：

    写法1：传参数，然后在方法里定义源生代码：ev.preventDefault();
    写法2[推荐]：@contextmenu.prevent  //右键点击会被阻止默认事件
    
### 2-4.键盘类事件：

读取键码的vue示例：

    vue代码：
    var first = new Vue({
		el:"#box",
		data:{
                arr:["xu", "zhang"],
                json:{a:"xufu", b:"fuping"}
		},
		methods:{
			show:function(ev) {
			    console.log(ev.keyCode);
			}
		}
	});
	
    相应html片段：
    <div id="box">
		<input type="text" @keydown="show($event)">
	</div>
	
keydown(按下), keyup(弹起)

常用键：

    回车：@keyup.13="show()"
          @keyup.enter="show()"
    
    上下左右：@keyup.up, @keyup.down, @keyup.left, @keyup.right

## 3.属性
v-bind

    <img v-bind:src="url" alt="">
    
    另一种写法：
    <img :src="url" alt="">