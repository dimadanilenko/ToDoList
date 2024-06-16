<script>
export default{
  data(){
    return{
      list:[],
      list: JSON.parse(localStorage.getItem('fruits')),
      result: [],
      error: '',
      inputText: '',
    }
  },
  created() {      
    const storedItems = JSON.parse(localStorage.getItem('fruits'));
    if (storedItems) {
      this.list = storedItems;
    }
  },
  methods:{
    writeInfo(index){
      if(this.inputText == ''){
        this.error = 'Введіть задачу!' 
      }else{
        this.list.unshift({ text: this.inputText, isActive: false })
        this.inputText = ''
        this.error = ''
        this.saveItems()
        this.result = JSON.parse(localStorage.getItem('fruits')) 
      }
    },
    deleteTask(index, item){
      this.list.splice(index, 1)
      this.saveItems()
    },
    doneTask(index){
      this.list[index].isActive = !this.list[index].isActive;
      this.saveItems()
    },
    corectTask(item, index){
      let newText = prompt('Введіть відредаговану задачу', index.text)
      if(newText){
        this.list.splice(item, 1, { text: newText, isActive: false })
        this.saveItems()
        console.log(item, index)
      }  
      
    },
    saveItems(){
      localStorage.setItem('fruits', JSON.stringify(this.list));
    }
  }
}
</script>

<template>
  <h2>Мої задачі</h2>
  <div className="main">
    <div className="add">
      <input v-model="inputText" @keyup.enter="writeInfo()" type="text" placeholder="Введіть задачу" maxlength="27">
      <img @click="writeInfo()" src="././assets/free-icon-plus-circular-button-64522.svg">
    </div>
    <p className="error">{{ error }}</p>
    <div v-for="(item, index) in list" className="task" :key="index" >
      <p :class="{ active: item.isActive }"><button @click="doneTask(index)" class="taskDone" :class="{ done: item.isActive }" ></button>{{item.text}}</p>
      <div className="buttons">
        <img @click='corectTask(index, item)' className='corect' src="././assets/353430-checkbox-edit-pen-pencil_107516.svg">
        <img @click="deleteTask(index, item)" className="delete" src="././assets/free-icon-garbage-bin-63481.png">
      </div>
    </div>  
  </div>
</template>

<style scoped>
h2{
  display: flex;
  justify-content: center;
  width: 500px;
  margin: 0 auto;
  margin-top: 20px;
  padding: 10px 40px;
  color: #DAF1FFFF;
  font-size: 60px;
  font-family: Helvetica, sans-serif;
  border-radius: 40px; 
  cursor: default;
}
.main{
  width: 900px;
  height: 100%;
  margin-top: 40px;
}
.main .error{
  margin-left: 20px;
  font-size: 20px;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: 500;
  color: #C80000FF;
}
.main .add{
  display: flex;
  justify-content: space-between;
}
.main .add input{
  width: 700px;
  height: 40px;
  margin: 20px 0px 0px 20px;
  font-size: 23px;
  border: none;
  outline: 0;
  border-radius: 5px;
  background-color: #FFFFFF8C;
  transition-property: background-color;
  transition-duration: 0.3s;
  cursor: pointer;
}
.main .add input:focus{
  background-color: #FFFFFFD5;
}
.main .add img{
  width: 50px;
  height: 50px;
  margin: 20px 20px 0px 0px;
}
.main .task{
  background-color: #C0AE8C;
  width: 850px;
  height: 50px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 20px;
  padding: 0px 20px;
  box-sizing: border-box;
  border-radius: 5px
  
}
.main .task p{
  font-size: 20px;
  font-family: Helvetica, sans-serif;
}
.main .task .buttons{
  display: flex;
  align-items: center;
}
.main .task .delete{
  width: 20px;
  height: 20px;
  
}
.main .task .corect{
  width: 30px;
  height: 30px;
}
.main .task .taskDone{
  width: 15px;
  height: 15px;
  margin-right: 10px;
  border: none;
  outline: none;
  border-radius: 2px;
}
.active{
  text-decoration: line-through;
}
.done{
  
  background-image: url(././assets/done.svg);
  background-size: 15px;
  
}
@media screen and (max-width: 800px) {
  .main{
    width: 400px;
  }
  .main .add{
    width: 400px;
    align-items: center;
  }
  .main .add input, .main .add img{
    margin: 0;
  }
  .main .task{
    width: 400px;
  }
  h2{
    margin: 0;
    padding: 0;
    font-size: 30px;
    width: 400px;
    margin-top: 40px;
  }
}
</style>
