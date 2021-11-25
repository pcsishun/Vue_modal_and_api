<template>
  <div id="app">
    <TitleName class="header-title"/>
    <div class="title-contant">
      <h1 v-if="titleData === null" class="loading">Loading...</h1>
      <div v-if="titleData !== null" class="title-element">
        <div v-for="(data,idx) in titleData" :key="idx" class="container contant-card">
          <ul>
            <li><h3>{{data.title}}</h3></li>
            <li><h6>{{data.body}}</h6></li>
            <li class="btn-comment"><button class="btn btn-primary" @click="ShowCommenet(data.id)">Show Comment</button></li>
          </ul>
        </div>
      </div>
    </div>
    <div id="myModal" class="modal"  :style="{ 'display': isShowComment ? 'block' : 'none' }"> <!-- show เป็น block ไม่ show เป็น none -->

      <!-- Modal content -->
      <div class="modal-content">
        <h4 v-if="commentData === null" style="text-align: center;">Loading...</h4>
        <h4 v-if="commentData !== null" style="text-align: center;">Comment</h4>
        <span class="close" @click="closePopup">&times;</span>
        <div v-for="(data, idx) in commentData" :key="idx">
          <ul  class="popup-container">
            <li><h5>Name: {{ data.name }}</h5></li>
            <li><h6>Email: {{ data.email }}</h6></li>
            <li><h6>Comment: {{ data.body }}</h6></li>
          </ul>
        </div>
      </div>

    </div>

  </div>
</template>

<script>
import TitleName from './components/TitleName.vue'

export default {
  name: 'App',
  components: {
    TitleName
  },
  data(){
    return{
      titleData: null,
      commentData: null,
      isShowComment: false,
    }
  },
  methods:{
    titleContent(){
      fetch('https://jsonplaceholder.typicode.com/posts').then( (res) =>{
        return res.json()
      }).then( (data) =>{
        this.titleData = data
      }).catch( (err) => {
        this.titleData = err.message
      })
    },
    ShowCommenet(ContantID){
      console.log(ContantID);
      this.isShowComment = true
      fetch(`https://jsonplaceholder.typicode.com/posts/${ContantID}/comments`).then( (res) => {
        return res.json()
      }).then((data) => {
        console.log(data)
        this.commentData = data
      }).catch( (err) => {
        this.commentData = err.message
      })
    },
    closePopup(){
      this.isShowComment = false
      this.commentData = null
    }
  },
  mounted(){
    this.titleContent();
  }
}
</script>

<style>
body{
  margin:0;
  padding:0;
}

.header-title{
  text-align:center;
}

.loading{
  text-align: center;
  margin-top: 15%;
}

ul{
  list-style-type:none;
}

.contant-card{
  background-color:rgb(191, 196, 196);
  border-radius: 5px;
  margin-top: 20px;
  height: 10rem;
}

.btn-comment{
  margin-top: 20px;
}

.popup-comment{
  position:absolute;
  top: 20px;
  left: 50%;
  height: 400px;
  width: 900px;
  border: 1px solid red;
 
}


/* The Modal (background) */
.modal {
  display: none; /* Hidden by default   none block*/
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content/Box */
.modal-content {
  background-color: #fefefe;
  margin: 15% auto; /* 15% from the top and centered */
  padding: 20px;
  border: 1px solid #888;
  width: 80%; /* Could be more or less, depending on screen size */
}

/* The Close Button */
.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}


.popup-container{
  background-color: rgb(177, 175, 174);
  border-radius: 15px;
}
</style>
