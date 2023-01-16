<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodoItem="addOneItem"></TodoInput>
    <TodoList 
    v-bind:propsdata="todoItems" 
    v-on:removeItem="removeOneItem"
    v-on:toggleItem="toggleOneItem"></TodoList>
    <TodoFooter v-on:clearAllItems="clearAllItems"></TodoFooter>
    
  </div>
</template>

<script>


import TodoHeader from './components/TodoHeader.vue' 
import TodoInput from './components/TodoInput.vue' 
import TodoList from './components/TodoList.vue' 
import TodoFooter from './components/TodoFooter.vue' 


export default {
  components: {
    'TodoHeader' : TodoHeader,
    'TodoInput': TodoInput,
    'TodoList': TodoList,
    'TodoFooter': TodoFooter,
  },

  data: function(){
    return {
      todoItems: []
    }
  },
  methods: {
    addOneItem : function(todoItem){
      var obj = { completed: false, item: todoItem }
      localStorage.setItem(todoItem, JSON.stringify(obj));
      this.todoItems.push(obj);

    }, 

    removeOneItem: function(todoItem, idx) {
      localStorage.removeItem(todoItem.item);
      this.todoItems.splice(idx, 1);
    },

    toggleOneItem: function (todoItem, idx) {      
      this.todoItems[idx].completed = !this.todoItems[idx].completed;
      localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    },
    clearAllItems: function() {
      localStorage.clear();
      this.todoItems = [];
    }

  },
  //Vue 생성자 
  created: function () {
    if (localStorage.length > 0) {
      for (var i = 0; i < localStorage.length; i++) {
        if (localStorage.key(i) !== 'loglevel:webpack-dev-server') {
          // console.log(JSON.parse(localStorage.getItem(localStorage.key(i))));
          // localStorage.getItem(JSON.parse(localStorage.getItem(localStorage.key(i))));
          this.todoItems.sort();
          this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
        }

      }
    }
  },
}
</script>

<style>

body {
  text-align: center;
  background-color: #f6f6f6;
}

input {
  border-style: groove;
  width: 200px;
}

botton {
  border-style: groove;
}

.shadow {
  box-shadow: 5px 10px 10px rbga(0,0,0,0.03)
}
</style>
