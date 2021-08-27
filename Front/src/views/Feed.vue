
<template>
<div id="global">
  <div id="container">
    <br/>
    <h2>Insira um comentário</h2><br>

    <input type="text" id="inputs_form" placeholder="Comentar" v-model="post_coment"/><br/>
    <input type="text" id="inputs_form" placeholder="Id do post" v-model="id"/><br/>
    
    <button @click="inserirComent" id="button_inserir">Postar</button><br/><br/><br/>
    
  <div id="post">
    <div id=""> {{ fetchComents()}} </div>
    <div id=""> {{ fetchPosts()  }} </div>

    <ul>
      <li v-for="post in posts" :key="post.id">
        <div>    {{fetchByIdPosts()}}  </div>
        Texto:   {{ post.text }}       <br/>
        Data:    {{ post.date }}       <br/>
        Horário: {{ post.time }}       <br/>
        <img
          v-if="post.id"
          :src="'http://localhost:3000/uploads/posts/' + post.id + '?' + Math.random()"
          width="200"
          height="150"
          alt="Nada"
          srcset=""
        />
        <ul>
            <li v-for="coment in coments" :key="coment.id">
              <div v-if="coment.postId === post.id">
                Comentário: {{ coment.post_coment }} <br/> 
              </div> 
            </li>
        </ul>
        <br/>
      </li>
    </ul>
  </div>
    <!-- <div class="divisao">
      <div id="posts"> {{ fetchComents()}} </div>
      <div id="posts"> {{ fetchPosts()  }} </div>
      <div id="posts">
        <ul>
          <li v-for="post in posts" :key="post.id">
            <div>    {{fetchByIdPosts()}}  </div>
            Texto:   {{ post.text }}       <br/>
            Data:    {{ post.date }}       <br/>
            Horário: {{ post.time }}       <br/>
            <img
              v-if="post.id"
              :src="'http://localhost:3000/uploads/posts/' + post.id + '?' + Math.random()"
              width="200"
              height="150"
              alt="Nada"
              srcset=""
            />
            <ul>
                <li v-for="coment in coments" :key="coment.id">
                  <div v-if="coment.postId === post.id">
                    Comentário: {{ coment.post_coment }} <br/> 
                  </div> 
                </li>
            </ul>
            <br/>
          </li>
        </ul>
      </div>
    </div> -->
  </div>
</div>
</template>

<script>

import axios from "axios";

export default {
  name: "Feed",
  data() {
    return {
      id: "",
      userId: "",
      text: "",
      date: "",
      time: "",
      post_coment: "",
      posts: [],
      coments: [],
      post: "",
      baseURI: "http://localhost:3000/posts",
      baseComents: "http://localhost:3000/coments",
      baseUpload: "http://localhost:3000/upload",
    };
  },
  methods: {
    fetchPosts: function() {
      axios.get(this.baseURI).then((result) => {
        console.log(result);
        this.posts = result.data;
      });
    },
    fetchByIdPosts: function() {
      axios.get(this.baseURI + "/" + this.post.id).then((result) => {
        console.log(result);
        this.post = result.data;
      });
    },
    inserirComent: function() {
      axios
        .post(this.baseComents, {
          postId: this.id,
          post_coment: this.post_coment,
        })
        .then((result) => {
          if (result.status == 201) {
            alert("Inserido com sucesso !!");
          }
        })
        .catch((error) => {
          if (error.response.status == 400) {
            alert("Dados incorretos !!");
          } else {
            alert("Problema desconhecido !!");
          }
        });
    },
    fetchComents: function() {
      axios.get(this.baseComents).then((result) => {
        console.log(result);
        this.coments = result.data;
      });
    },
  },
};

</script>

<style>
h2{
  color: rgb(141, 141, 141);
  font-size: 28px;
  background-color: transparent;
  margin-bottom: -1em;
  padding: 1%;
}
/* .divisao{
  margin-left: -6000rem;
} */
/* #container{
  position: fixed;
  left: 50em;
} */
#inputs_form{
  width: 300px;
  /* height: 27px; */
  font-size: 16px;
  border-top-color: transparent;
  border-bottom-color:#24642e;
  border-left-color: transparent;
  border-right-color: transparent;
  
  color: rgb(85, 85, 85);
  background-color: transparent;
  transition: background-color 0.2s;
  margin-top: 1em;
}
#inputs_form:hover{
  outline: none;
  /* margin-top:-0.2px; */
  border-bottom:3px solid #399c48;
}
#inputs_form:focus{
  outline: none;
  border-bottom:3px solid #399c48;
}
#inputs_login ::placeholder{
 color:rgba(0, 66, 0, 0.507);
}

#buttonPostar{
  margin-top:-2em;
  background-color: rgb(156, 255, 117);
  transition: background-color 0.2s;
}
#buttonPostar:hover{
  transition: background-color 0.2s;
  transition: color 0.2s;
  background-color: rgb(122, 255, 70);
}
#posts{
  position: absolute;
  left: 20em;
}
</style>


