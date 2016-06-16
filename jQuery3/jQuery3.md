**1、简化了 show/hide**
之前的 show/hide 是大兼容，比如 show， 无论元素的 display 是写在style，stylesheet里都能显示出来。3.0 则不同了，
写在 stylesheet 里的 display:none 调用 show 后仍然隐藏。 3.0 建议采用 class 方式去显示隐藏，或者完全采用 hide 先隐藏（不使用css代码），再调用 show 也可以。

<style>
    input {
        display: none;
    }
</style>
<input id="txt" type="text" value=""/>
<script>
    $('#txt').show(); // 仍然隐藏的状态
</script>


