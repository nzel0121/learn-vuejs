<template>
  <div id="app">
   <TodoHeader/>
   <!-- <TodoInput v-on:하위컴포넌트에서 발생시킨 이벤트이름="현재컴포넌트의 메서드 명"> -->
   <TodoInput v-on:addTodoItem="addOneItem"/>
   <TodoList v-bind:propsdata="todoItems" 
      v-on:removeItem="removeOneItem" 
      v-on:toggleItem="toggleOneItem"/>
   <TodoFooter v-on:clearAll="clearAllItems"/>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  data: function(){
    return {
      todoItems:[]
    }
  },
  methods:{
    addOneItem: function(newTodoItem){
      var obj = {completed: false, item: newTodoItem}
      localStorage.setItem(newTodoItem, JSON.stringify(obj));
      this.todoItems.push(obj);
    },
    removeOneItem: function(todoItem, index){
      console.log('remove Items',todoItem, index);
      localStorage.removeItem(localStorage.key(index));
      this.todoItems.splice(index,1);
    },
    toggleOneItem: function(todoItem, index){
      console.log(index);
      //todoItem.completed = !todoItem.completed;
      this.todoItems[index].completed = !this.todoItems[index].completed;
      localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    },
    clearAllItems: function(){
      localStorage.clear();
      this.todoItems = [];
    }
  },
  created: function(){
    if(localStorage.length > 0){
        for(var i=0;i< localStorage.length; i++){
            // console.log(localStorage.key(i));
            if(localStorage.key(i) !== 'loglevel:webpack-dev-server'){
              this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
            }
        }
    }
  },
  components:{
    'TodoHeader' : TodoHeader,
    'TodoInput' : TodoInput,
    'TodoList' : TodoList,
    'TodoFooter' : TodoFooter
  }
}
</script>

<style>
body{
  text-align: center;
  background-color: #f6f6f6; 
}
input {
  border-style: groove;
  width: 200px;
}
button{
  border-style: groove;
}
.shadow{
  box-shadow: 5px 10px 10px rgba(0,0,0,0.03);
}
</style>
