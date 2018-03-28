<template>
  <div id="app">
      <h1>{{title}}</h1>
      <input type="text" name="todo" v-model="todo">
      <button v-on:click="addTodo()">添加</button>
      <ul>
          <li v-for="item in items" :class="{finished: item.isFinished}" v-on:click="goFinish(item)">{{item.label}}</li>
      </ul>
  </div>
</template>

<script>
//export命令定义了模块的对外接口以后，其他JS文件就可以通过import命令加载这个模块
import Store from './store.js'
export default {
    data: function() {
        return {
            title: 'this is a todo list',
            items: [{
                label: 'coding',
                isFinished: false,
            },{
                label: 'walking',
                isFinished: true
            }],
            todo: ''
        }
    },
    watch: {
        'items': {
            handler: function(val, oldVal){
                Store.save(val);
            },
            deep: true
        }
    },
    methods: {
        goFinish: function(item){
            item.isFinished = !item.isFinished;
        },
        addTodo: function(){
            this.items.push({
                label: this.todo,
                isFinished: false
            })
            this.todo = "";
        }
    }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.finished {
    text-decoration: underline;
}
</style>
