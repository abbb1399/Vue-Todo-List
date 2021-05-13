<template>
  <div class="hello">
    <h3>{{ message }}</h3>
    <form name="todo-form" method="post" action="" v-on:submit.prevent="addTask"> 
      <input name="add-todo" type="text" v-model="addTodoInput" :class="{error:hasError}" />
      <button type="submit">Add</button>
    </form>

    <div class="todo-lists" v-if="lists.length">
      <h3>My Todo Tasks</h3>
      <ul>
        <li v-for="list in filterLists" :key="list.id">
          <input type="checkbox" @change="completeTask(list)" v-bind:checked="list.isComplete"/>
          <span 
            class="title" 
            contenteditable="true"
            @keydown.enter="updateTask($event,list)"
            @blur="updateTask($event,list)"
            :class="{completed: list.isComplete}"
          >
            {{list.title}}
          </span>
          <span class="remove" v-on:click="removeTask(list)">x</span>
        </li>
      </ul>
    </div>

  </div>
</template>

<script>
import _ from 'lodash';

export default {
  data() {
   return{
     message: 'Welcome to Todo App',
     addTodoInput: '',
     lists: [],
     hasError: false      //에러 핸들링
   }
  },
  computed:{
    filterLists(){
      return _.orderBy(this.lists, ['isComplete', false])
    }

  },

  methods:{
    addTask(){  
      // 값이 없으면 hasError 를 true로
      if(!this.addTodoInput){
        this.hasError = true;
        return;
      }

      this.hasError = false; // 내용이 있으면 hasError를 false로 

      this.lists.push({
        id: this.lists.length+1,
        title: this.addTodoInput,
        isComplete: false
      });

      this.addTodoInput = ''; // 검색후 초기화
    },

    updateTask(e,list){
      e.preventDefault();
      list.title = e.target.innerText;
      e.target.blur();
    },

    completeTask(list){
      list.isComplete = !list.isComplete
    },

    removeTask(list){
      console.log(list)

      var index = _.findIndex(this.lists, list);
      this.lists.splice(index,1);
    }


  }


}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
:root{font-family: Arial;}
input[type=text]{
  font-size:16px;
  padding: 8px;
  border-radius: 10px;
  border: 1px solid #c4c4c4;
}

button{
  background: #3498db;
  background-image: linear-gradient(to bottom, #3498db, #2980b9);
  border-radius: 28px;
  
  color: #ffffff;
  font-size: 16px;
  padding: 8px 20px;
  border: none;
  cursor:pointer;
}
button:hover {
  background: #3cb0fd;
  background-image: linear-gradient(to bottom, #3cb0fd, #3498db);
}
input[type=text].error{border: 1px solid red;}
[contenteditable=true]:focus{
  
  
  overflow: hidden;
  border: 1px solid transparent;

  -webkit-appearance: textfield;
  -moz-appearance: textfield;
  appearance: textfield;

  white-space: nowrap;
  border-radius: 10px;
}

.title{
  display: inline-block;
  width: 200px;
  border: 1px solid transparent;
  padding: 8px;
  font-size: 16px;
  vertical-align:middle;
}

.title:hover{
  border:1px solid #c4c4c4;
  border-radius: 10px;
}

.remove{
  cursor:pointer;
  display:inline-block;
  border: 1px solid #c4c4c4;
  border-radius: 50%;
  padding:0px 4px;
}
.remove:hover{
  background: #3cb0fd;
}

.completed{
  text-decoration: line-through;
}
</style>
