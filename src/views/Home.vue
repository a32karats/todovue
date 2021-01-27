<template>
  <div id="app">
    <div class="frame">
      <h2>Todo List</h2>
      <div class="basic">
        <input type="text" v-model="Todo" />
        <button @click="addInput" type="submit">追加</button>
      </div>
      <div class="form">
        <div class="addform" v-for="(text, index) in data" :key="index">
          <input type="text" v-model="text.todo" />
          <button class="updatebutton" @click="updateInput(index)">更新</button>
          <button class="removebutton" @click="removeInput">削除</button>
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
      data: [],
    };
  },
  
  methods: {
    addInput() {
      console.log(this.Todo)
      // バックエンドのAPIはポート番号が8000です
      axios.post("http://localhost:8000/api/todo", {
        todo: this.Todo,
      })
      .then((res) => {
        console.log(res);
        this.Todo = "";
      }).catch((err) => {
        console.log(err)
      });
    },
    removeInput() {
      axios.delete("http://localhost:8000/api/todo" + this.data)
        .then((res) => {
          console.log(res);
        });
    },
    updateInput(index) {
      // バックエンドのAPIはポート番号が8000です
      const todo = this.data[index].todo;
      const id = this.data[index].id;

      axios.put(`http://localhost:8000/api/todo/${id}`, {
        todo: todo,
      }).then((res) => {
        console.log(res);
      });
    },
  },
  mounted() {
    // バックエンドのAPIはポート番号が8000です
    axios.get("http://localhost:8000/api/todo").then((res) => {
      // thisで使う場合はdataプロパティに予め定義する必要がある
      // また今回はdataで表示させる処理を行っているのでthis.dataで良い
      console.log(res);
      this.data = res.data.data;
    });
  },
};
</script>


<style scoped>
element.style {
  user-select: auto;
}

html {
  position: relative;
}
.frame {
  width: 30%;
  background-color: white;
  border-radius: 10px;
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
  width: 45%;
  height: 20%;
}

h2 {
  position: absolute;
  left: 5%;
  top: 20%;
  color: black;
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
  border-color: #cccccc;
  border-style: solid;
  border-width: 1px;
}

.basic button {
  color: #cd70fa;
  margin-left: 50px;
  padding: 10px 20px;
  border-radius: 8px;
  border-color: #cd70fa;
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
  border-color: #cccccc;
  border-style: solid;
  border-width: 1px;
  color: black;
}

.updatebutton {
  color: #fa9870;
  margin-left: 270px;
  padding: 10px 20px;
  border-radius: 8px;
  border-color: #fa9870;
  border-style: solid;
  border-width: 2px;
  background-color: white;
  cursor: pointer;
}

.removebutton {
  color: #70fadc;
  margin-left: 10px;
  padding: 10px 20px;
  border-radius: 8px;
  border-color: #70fadc;
  border-style: solid;
  border-width: 2px;
  background-color: white;
  cursor: pointer;
}

.basic button:hover {
  background: #cd70fa;
  color: white;
}

.updatebutton:hover {
  background: #fa9870;
  color: white;
}

.removebutton:hover {
  background: #70fadc;
  color: white;
}
</style>