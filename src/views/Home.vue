<template>
  <div id="app">
    <div class="frame">
      <h2>Todo List</h2>
      <div class="basic">
        <input type="text" v-model="Todo">
        <button @click="addInput" type="submit">追加</button>
     </div>
      <div class="form">
         <div class="addform" v-for="(text,id) in texts" :key="id">
          <input type="text" v-model="text[id]">
          <button class="updatebutton" @click="updateInput(text.id)">更新
           </button>
           <button class="removebutton" @click="removeInput(text.id)">削除
           </button>
        </div>
     </div>
   </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
 data() {
   return {
     Todo: "",
     texts: [],
   };
},
 async created() {
   let data = await axios.get("http://localhost:8080/");
   this.texts = data.texts;
 },
methods: {
  addInput: function() {
    axios.post("http://localhost:8080/")
      .then(res => {
        this.texts = res.data;
        this.texts.push(this.newItem);
        this.newItem = "";
      })
  },
  removeInput(index) {
    this.texts.splice(index,1);
  },
  updateInput: function() {
    axios.put("http://localhost:8080/").then(() => {
   this.getTodoList()}
   )},
},
  mounted() {
    axios.get('/').then(res =>{this.texts = res.data;})
}};
</script>


<style scoped>
html {
  position:relative;
}
.frame {
 width:30%;
 background-color: white;
 border-radius: 10px;
 position:absolute;
 top: 0;
 right: 0;
 bottom: 0;
 left: 0;
 margin: auto;
 width: 45%;
 min-height: 20%;
 height: auto !important;
 height: 20%;
}

h2 {
  position:absolute;
  left: 5%;
  top: 20%;
  color:black;
  font-size: 30px;
}

.basic {
  margin-top: 80px;
  margin-left: 35px;
}

.basic input {
  height: 40px;
  width: 75%;
  border-radius: 8px;
  color: black;
  text-align: left;
  border-color: #CCCCCC;
  border-style: solid;
  border-width: 1px;
}

.basic button {
  color: #CD70FA;
  margin-left: 50px;
  padding: 10px 20px;
  border-radius: 8px;
  border-color: #CD70FA;
  border-style: solid;
  border-width: 2px;
  background-color: white;
  cursor: pointer;
}

.form {
  margin-top: 15px;
  margin-left: 35px;
}

.addform {
  margin-bottom: 5px;
}

.form input {
  height: 40px;
  width: 30%;
  border-radius: 8px;
  border-color:  #CCCCCC;
  border-style: solid;
  border-width: 1px;
  color: black;
}

.updatebutton {
  color: #FA9870;
  margin-left: 270px;
  padding: 10px 20px;
  border-radius: 8px;
  border-color: #FA9870;
  border-style: solid;
  border-width: 2px;
  background-color: white;
  cursor: pointer;
}

.removebutton {
  color: #70FADC;
  margin-left: 10px;
  padding: 10px 20px;
  border-radius: 8px;
  border-color: #70FADC;
  border-style: solid;
  border-width: 2px;
  background-color: white;
  cursor: pointer;
}

.basic button:hover {
  background: #CD70FA;
  color: white;
}

.updatebutton:hover {
  background: #FA9870;
  color: white;
}

.removebutton:hover {
  background: #70FADC;
  color: white
}
</style>