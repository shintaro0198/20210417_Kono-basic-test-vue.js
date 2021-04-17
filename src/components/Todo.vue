<template>
  <div id="todo">
    <h1>Todo List</h1>
    <div class="todo-add flex">
      <input type="text" v-model="text">
      <div class="button purple" @click="add">
        <p>追加</p>
      </div>
    </div>
    <div v-for="item in list" :key="item.id" class="todo-content flex">
      <input type="text" v-model="item.content">
      <div class="flex todo-content-block">
        <div class="button orange" @click="update(item.id,item.content)">
          <p>更新</p>
        </div>
        <div class="button green" @click="destroy(item.id)">
          <p>削除</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data(){
    return {
      list : [],
      text : ""
    }
  },
  methods:{
    add(){
      axios.post('https://nameless-bastion-19637.herokuapp.com/api/todo', {
        content : this.text
      })
      .then(()=>{
        this.text = ""
        this.load()
      })
    },
    update(id,text){
      axios.put('https://nameless-bastion-19637.herokuapp.com/api/todo/' + id,{
        content : text
      })
      .then(()=>{
        this.load()
      })
    },
    destroy(id){
      axios.delete('https://nameless-bastion-19637.herokuapp.com/api/todo/' + id)
      .then(()=>{
        this.load()
      })
    },
    load(){
      axios.get('https://nameless-bastion-19637.herokuapp.com/api/todo')
      .then(response=>{
        this.list = []
        for(let i = 0; i<response.data.data.length;i++){
          const task = response.data.data[i]
          this.list.push(task)
        }
      })
    }
  },
  async created(){
    axios.get('https://nameless-bastion-19637.herokuapp.com/api/todo')
    .then(response=>{
      for(let i = 0; i<response.data.data.length;i++){
        const task = response.data.data[i]
        this.list.push(task)
      }
    })
  },

}
</script>

<style scoped>
#todo{
  background-color: white;
  padding: 28px 30px;
  border-radius: 10px;
}
input{
  border: 1px solid #CCCCCC;
  outline: none;
  font-size: 14px;
  padding: 0 5px;
}
.button{
  width: 57px;
  font-size: 12px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: bold;
  cursor: pointer;
}
input,.button{
  border-radius: 5px;
}
.flex{
  display: flex;
  justify-content: space-between;
}
h1{
  font-size: 24px;
  font-weight: 600;
  margin-bottom: 15px;
}
.purple{
  border: 2px solid #DF7BFA;
  color:  #DF7BFA;
}
.orange{
  border: 2px solid #FA9972;
  color:  #FA9972;
  margin-right: 5px;
}
.green{
  border: 2px solid #71FADC;
  color:  #71FADC;
}
.purple:hover{
  background-color: #DF7BFA;
  color: white;
  transition: 0.5s;
}
.orange:hover{
  background-color: #FA9972;
  color: white;
  transition: 0.5s;
}
.green:hover{
  background-color: #71FADC;
  color: white;
  transition: 0.5s;
}
.todo-add{
  line-height: 35px;
  margin-bottom: 20px;
}
.todo-add input{
  width: 81%;
}
.todo-content{
  line-height: 35px;
  margin-bottom: 5px;
}
.todo-content input{
  width: 45%;
}
@media screen and (max-width:717px){
  .todo-add{
    line-height: 50px;
  }
}
@media screen and (max-width: 390px){
  #todo{
    height: auto;
  }
  .todo-content input{
    line-height: 70px;
  }
  .todo-content-block{
    display: block;
    margin-right: auto;
  }
}
</style>