**1、简化了 show/hide**
之前的 show/hide 是大兼容，比如 show， 无论元素的 display 是写在style，stylesheet里都能显示出来。3.0 则不同了，
写在 stylesheet 里的 display:none 调用 show 后仍然隐藏。 3.0 建议采用 class 方式去显示隐藏，或者完全采用 hide 先隐藏（不使用css代码），再调用 show 也可以。

    input {
        display: none;
    }

<input id="txt" type="text" value=""/>
$('#txt').show(); // 仍然隐藏的状态

**2.data 方法**
jQuery 3 将会把所有属性键名转换成驼峰形式
jQuery 3 以前的版本时，如果运行如下代码：
var $elem = $('#container');

$elem.data({
   'my-property': 'hello'
});

console.log($elem.data());
将会在控制台得到如下结果：
{my-property: "hello"}
而在 jQuery 3 中，我们将会得到如下结果：
{myProperty: "hello"}

**3.for...of 循环**
循环体内每次拿到的值并不是一个 jQuery 对象，而是一个 DOM 元素（译注：这一点跟 .each() 方法类似）
var $inputs = $('input');
for(var i = 0; i < $inputs.length; i++) {
   $inputs[i].id = 'input-' + i;
}
<!----------jQuery3------------->
var $inputs = $('input');
var i = 0; 

for(var input of $inputs) {
   input.id = 'input-' + i++;
}

**4.unwrap() 方法**
unwrap([selector])
有了这个特性，你就可以给这个方法传入一个包含选择符表达式的字符串，用它来在父元素内进行匹配。如果存在匹配的子元素，则这个子元素的父层将被移除；
如果没有匹配，则不进行操作。

**5.:visible 和 :hidden**
Query 3 将会修改 :visible 和 :hidden 过滤器的含义。
只要元素具有任何布局盒，哪怕宽高为零，也会被认为是 :visible。举个例子，br 元素和不包含内容的行内元素现在都会被 :visible 这个过滤器选中。
因此，如果你的页面中包含如下的结构：
<div></div>
<br/>

然后运行以下语句：
console.log($('body :visible').length);
在 jQuery 1.x 和 2.x 中，你得到的结果会是 0；但在 jQuery 3 中，会得到 2。


**6.Deferred 对象**

var deferred = $.Deferred();

deferred
  .then(function() {
    throw new Error('An error');
  })
  .then(
    function() {
      console.log('Success 1');
    },
    function() {
      console.log('Failure 1');
    }
  )
  .then(
    function() {
      console.log('Success 2');
    },
    function() {
      console.log('Failure 2');
    }
  );

deferred.resolve();

在 jQuery 1.x 和 2.x 中，只有第一个函数（也就是抛出错误的那个函数）会被执行到。
此外，由于我们没有为 window.onerror 定义任何事件处理函数，控制台将会输出 “Uncaught Error: An error”，而且程序的执行将中止。

而在 jQuery 3 中，整个行为是完全不同的。
你将在控制台中看到 “Failure 1” 和 “Success 2” 两条消息。那个异常将会被第一个失败回调处理，并且，一旦异常得到处理，那么后续的成功回调将被调用。

**7.已废弃、已移除的方法和属性**
废弃 bind()、unbind()、delegate() 和 undelegate() 方法
jQuery 3 终于开始将这些方法标记为 “废弃” 了，并计划在未来的某个版本（很可能是 jQuery 4）中将它们彻底移除。
因此，请在你的项目中统一使用 on() 和 off() 方法

jQuery 3 彻底抛弃了 load()、unload() 和 error() 等已经标记为废弃的方法。这些方法在很早以前（从 jQuery 1.8 开始）就已经被标记为废弃了，但一直没有去掉。
如果你正在使用的某款插件仍然依赖这些方法，那么升级到 jQuery 3 会把你的代码搞挂。

jQuery 3 彻底抛弃了 context、support 和 selector 等已经标记为废弃的属性。

**8.width() 和 height() 的返回值将不再取整**

<div class="container">
   <div>My name</div>
   <div>is</div>
   <div>Aurelio De Rosa</div>
</div>

在 jQuery 3 以前的版本中，如果你尝试通过以下代码来获取子元素的宽度……

$('.container div').width();

……那么你得到结果将是 33。原因在于 jQuery 会把 33.33333 这个值取整。而在 jQuery 3 中，
这个 bug 已经被修复了，因此你将会得到更加精确的结果（即一个浮点数）。



