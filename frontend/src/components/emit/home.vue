<template>
    <div class="home">
        <h1>home</h1>
        <p>{{aboutMsg}}</p>
    </div>
</template>
<script>
    export default {
        data () {
            return {
                aboutMsg: '我是home组件'
            }
        }
    }
</script>
<<style scoped>
.home{
     position: absolute;
        top: 50%;
        left: 50%;
        bottom: 20px;
        margin-top: -150px;
        margin-left: -175px;
        width: 350px;
        min-height: 300px;
        padding: 50px;
        border-radius: 8px;
        border:2px solid;
        box-sizing: border-box;
    float:left;
        background-color: #fff;
}
.home p{
     color: red;
     text-align: left;
}
</style>
