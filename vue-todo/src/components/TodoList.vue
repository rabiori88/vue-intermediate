<template>
  <div>
    <ul>
      <li v-for="(todoItem, idx) in todoItems" v-bind:key="todoItem.item" class="shadow">
        <i class="fas fa-check checkBtn" 
        v-bind:class="{checkBtnCompleted: todoItem.completed}" 
        v-on:click="toggleComplete(todoItem, idx)"></i>
        <span v-bind:class="{ textCompleted: todoItem.completed }"> {{ todoItem.item }}</span> 
        <span class="removeBtn" v-on:click="removeTodo(todoItem, idx)">
          <i class="fas fa-trash-alt fa-flip"></i>
        </span>
      </li>      
    </ul>
  </div>
</template>

<script>
export default {

  data: function() {
    return {
      todoItems: []
    }
  },

  methods:{
    removeTodo: function(todoItem, idx) {

      localStorage.removeItem(todoItem);
      this.todoItems.splice(idx, 1);
      console.log(todoItem , idx);

    },
    toggleComplete: function(todoItem, idx) {

      console.log( idx);
      todoItem.completed = !todoItem.completed;
      //로컬 스토리지 데이터 갱신
      localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
      
    }
  },

  //Vue 생성자 
  created: function() {
    if(localStorage.length > 0) {
      for(var i = 0; i < localStorage.length; i++) {
        if(localStorage.key(i) !== 'loglevel:webpack-dev-server')
        {
          // console.log(JSON.parse(localStorage.getItem(localStorage.key(i))));
          // localStorage.getItem(JSON.parse(localStorage.getItem(localStorage.key(i))));
          this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
        }
        
      }
    }
  },


}
</script>

<style scoped>
ul{
  list-style-type: none;
  padding-left: 0px;
  margin-top: 0;
  text-align: left;
}

li {
  display: flex;
  min-height: 50px;
  height: 50px;
  line-height: 50px;
  margin: 0.5rem 0;
  padding : 0 0.0rem;
  background: white;
  border-radius: 5px;

}

.checkBtn {
  line-height: 45px;
  color: #62acde;
  margin-right: 5px;

}

.checkBtnCompleted {
  color: #b3adad;
}

.textCompleted{
  text-decoration: line-through;
  color: #b3adad;
}
.removeBtn {
  margin-left : auto;
  color: #de4343;
}

</style>