<template>
  <div class="header">
    <div class="container">
      <div class="header-box">
        <h4 class="header-title">ToDolist</h4>
        <div class="header-nav">
          <div class="header-btn"
           @click="changeAllNavItem()"
           :class="{active:all}"
           >Barchasi</div>
          <div class="header-btn"
           @click="changeTrueNavItem()"
           :class="{active:onlyTrue}"
           >Bajarilganlar</div>
          <div class="header-btn"
           @click="changeFalseNavItem()"
           :class="{active:onlyFalse}"
           >Bajarilmaganlar</div>
        </div>
      </div>
    </div>
  </div>

  <div class="main">
    <div class="container">
      <div class="main-box">
        <div class="main-cards" v-show="all">
          <div class="task-card"
            v-for="value of tasks"
            :class="{is_done: value.status == 'true'}"
          >
            <div class="task-card-caption">
              <h4 class="task-title">{{value.title}}</h4>
              <div class="task-card-btns">
                <button @click="openEditModal(value.id)"><img src="@/assets/edit-symbol.png" /></button>
                <button @click="deleteTask(value.id)"><img src="@/assets/remove-symbol.png" /></button>
              </div>
            </div>
            <p class="task-description">
              {{value.description}}
            </p>
            <div class="task-data">
              <h4 class="task-dedline">{{value.dedline}}</h4>
              <h4 class="task-author">{{value.author}}</h4>
            </div>
          </div>
        </div>

        <div class="main-cards" v-show="onlyTrue">
          <div class="task-card"
            v-for="value of tasks.filter(t => t.status == 'true')"
            :class="{is_done: value.status == 'true'}"
          >
            <div class="task-card-caption">
              <h4 class="task-title">{{value.title}}</h4>
              <div class="task-card-btns">
                <button @click="openEditModal(value.id)"><img src="@/assets/edit-symbol.png" /></button>
                <button @click="deleteTask(value.id)"><img src="@/assets/remove-symbol.png" /></button>
              </div>
            </div>
            <p class="task-description">
              {{value.description}}
            </p>
            <div class="task-data">
              <h4 class="task-dedline">{{value.dedline}}</h4>
              <h4 class="task-author">{{value.author}}</h4>
            </div>
          </div>
        </div>

        <div class="main-cards" v-show="onlyFalse">
          <div class="task-card"
            v-for="value of tasks.filter(t => t.status != 'true')"
            :class="{is_done: value.status == 'true'}"
          >
            <div class="task-card-caption">
              <h4 class="task-title">{{value.title}}</h4>
              <div class="task-card-btns">
                <button @click="openEditModal(value.id)"><img src="@/assets/edit-symbol.png" /></button>
                <button @click="deleteTask(value.id)"><img src="@/assets/remove-symbol.png" /></button>
              </div>
            </div>
            <p class="task-description">
              {{value.description}}
            </p>
            <div class="task-data">
              <h4 class="task-dedline">{{value.dedline}}</h4>
              <h4 class="task-author">{{value.author}}</h4>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="container">
    <div class="btn-box">
      <div class="add-btn" @click="openModal()">+</div>
    </div>
  </div>

  <div class="modal-container">
    <div
      class="modal-box"
      :class="{
        active: !modalBoxIsActive,
      }"
    >
      <div class="modal-caption">
        <img @click="openModal()" src="@/assets/remove-symbol.png" alt="">
      </div>
      <div>
        <label>Sarlavha</label>
        <input type="text" placeholder="Sarlavha" v-model="task.title" />
      </div>

      <div>
        <label>Izoh</label>
        <input type="text" placeholder="Izoh" v-model="task.description" />
      </div>

      <div>
        <label>Muddat</label>
        <input type="date" placeholder="Oxirgi muddat" v-model="task.dedline" />
      </div>
      <div class="div" v-show="!showModalBtn">
        <label>Status</label>
        <select name="" id="" v-model="task.status">
          <option value=true>Bajarilgan</option>
          <option value=false>Bajarilmagan</option>
        </select>
      </div>
      <div>
        <label>Xodim</label>
        <select name="" id="" v-model="task.author">
          <option value="Ochilov J">Ochilov J</option>
          <option value="Hamidov B">Hamidov B</option>
          <option value="Isoqov N">Isoqov N</option>
        </select>
      </div>
      <button v-show="showModalBtn" @click="addTask()">Add</button>
      <button v-show="!showModalBtn" @click="editTask()">Edit</button>
    </div>
  </div>

  <div
    :class="{
      blur_back:!modalBoxIsActive
    }"
  ></div>
  <!-- <div class="container">
    {{ task }}
  </div> -->
</template>







<script>
import axios, { all } from "axios";

export default {
  data() {
    return {
      url: "http://localhost:3000/tasks",
      task: { status: false},
      tasks: [],
      showModalBtn: true,
      modalBoxIsActive: true,
      modalIsEdit: false,
      all: true,
      onlyTrue: false,
      onlyFalse: false
    };
  },
  methods: {
    changeFalseNavItem(){
      this.all = false
      this.onlyTrue = false
      this.onlyFalse = true
    },
    changeAllNavItem(){
      this.onlyFalse = false
      this.onlyTrue = false
      this.all = true
    },
    changeTrueNavItem(){
      this.all = false
      this.onlyFalse = false
      this.onlyTrue = true
    },
    openModal() {
      this.modalBoxIsActive = !this.modalBoxIsActive;
    },

    openEditModal(taskId){
      axios.get(`${this.url}/${taskId}`)
      .then(res => {
        this.task = res.data
      })
      

      this.showModalBtn = !this.showModalBtn
      this.modalIsEdit = !this.modalIsEdit
      this.modalBoxIsActive = !this.modalBoxIsActive
    },

    editTask(){
      axios.put(`${this.url}/${this.task.id}`, this.task)
      .then(res =>{
        console.log(res);
        
      })
      this.openModal()
      this.getTasks()
    },
    addTask() {
      if(
        this.task?.title != '' && 
        this.task?.description != '' &&
        this.task?.dedline != null &&
        this.task?.author !=''
      ){
        this.tasks = [this.task, ...this.tasks];
  
        console.log(this.task);
        
        axios.post(this.url, this.task).then((res) => {
          console.log(res.data);
        });
      }else{
        return false
      }

      this.task = { status: false}
      this.openModal()
    },

    getTasks(){
      axios.get(this.url)
      .then(res =>{
        
        this.tasks = [...res.data]
      })

      

      
    },
    deleteTask(id){
      if(confirm("Ma'lumotni o'chirishga ishonchinggiz komilmi?")){
        axios.delete(`${this.url}/${id}`)
        .then(()=>{
          this.getTasks()
        })
      }
    }
  },
  mounted() {
    this.getTasks()
  },
};
</script>

<style>
/* Header styles */
template {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.container {
  margin-left: auto;
  margin-right: auto;
  max-width: 370px;
}
.header {
  border-bottom: 1px solid #000000;
  position: sticky;
  top: 0;
  background-color: #fff;
}
.header-title {
  font-size: 24px;
  font-family: Roboto;
  font-weight: 600;
  color: #000000;
  text-align: center;
  margin-bottom: 7px;
  margin-top: 30px;
}
.header-nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.header-btn {
  width: 33%;
  text-align: center;
  font-size: 12px;
  padding-top: 8px;
  padding-bottom: 8px;
  cursor: pointer;
}
.header-btn.active {
  background-color: red;
  color: #fff;
}

/* Main styles */
.main-box {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  padding-top: 60px;
}
.main-cards{
  display:flex;
  flex-direction: column;
  gap:10px;
  width:100%;
}

.task-card {
  padding: 12px 16px;
  background-color: #f4a4a9;
  height: 130px;
}
.task-card.is_done{
  background-color: #9fe3e8
}
.task-card-caption {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}
.task-title {
  font-size: 18px;
  font-weight: 700;
  margin: 0;
}
.task-description {
  font-size: 13px;
  font-weight: 400;
  margin-bottom: 8px;
  height: 45px;
}
.task-data {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.task-card-btns {
  display: flex;
  gap: 4px;
}
.task-card-btns button {
  cursor: pointer;
  background: none;
  border: none;
}
.task-card.not-done {
  background-color: #f4a4a9;
}

/* Fixed add-btn styles */
.add-btn {
  width: 58px;
  height: 58px;
  background-color: #2cb932;
  color: #fff;
  position: fixed;
  bottom: 10px;
  border-radius: 50%;
  font-size: 36px;
  font-weight: bolder;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.btn-box {
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Fixed modal styles */
.modal-box {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 20px;
  gap: 10px;
  border: 1px solid #000000;
  position: fixed;
  width: 370px;
  z-index: 1001;
  top: -1000px;
  background-color: #fff;
  transition: ease 0.4s;
}
.modal-box.active {
  top: 150px;
}
.modal-box div {
  width: 100%;
  display: flex;
  flex-direction: column;
}
.modal-box label {
  font-size: 14px;
  color: #646464;
  margin-left: 12px;
}
.modal-box input,
.modal-box select {
  padding: 8px;
  border: 1px solid #646464;
  outline: none;
  border-radius: 15px;
}
.modal-box button {
  background-color: #2cb932;
  padding: 10px 68px;
  border: none;
  border-radius: 15px;
  cursor: pointer;
}
.modal-box select{
  cursor: pointer;
}
.modal-container {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
}
.blur_back{
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgb(0, 0, 0, 0.7);
  z-index: 1000;
  backdrop-filter: blur(3px);
  /* display: block; */
}
.modal-caption{
  display: flex;
  align-items: end;
  justify-content: end;
}
.modal-caption img{
  width: 15px;
  height: 15px;
  cursor: pointer;
}

</style>
