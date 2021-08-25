
<template>

  <div id="container">
    <br/>
    <h2>Feed de Posts:</h2><br>

    Comentar: <input type="text" class="inputs_form" name="" id="1" v-model="post_coment"/> <br/>
    Id do post: <input type="text" class="inputs_form" name="" id="0" v-model="id"/> <br/>
    <button @click="inserirComent">Inserir</button><br/><br/><br/>
    <button @click="fetchComents">Buscar Coments</button> <br/><br/>

    <button @click="fetchPosts">Buscar Todos</button> <br/><br/>

    <ul>
      <li v-for="post in posts" :key="post.id">
        Texto: {{ post.text }} <br/>
        Data: {{ post.date }} <br/>
        Horário: {{ post.time }} <br/>
        <ul>
            <li v-for="coment in coments" :key="coment.postId">
              <div v-if="coment.postId === post.id">
                Comentário: {{ coment.post_coment }} <br/><br/> 
              </div> 
            </li>
        </ul>
        <br/>
      </li>
    </ul>

  </div>

</template>

<script>

import axios from "axios";

export default {
  name: "Feed",
  data() {
    return {
      id: "",
      text: "",
      date: "",
      time: "",
      post_coment: "",
      posts: [],
      coments: [],
      post: "",
      baseURI: "http://localhost:3000/posts",
      baseComents: "http://localhost:3000/coments",
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
      axios.get(this.baseURI + "/" + this.id).then((result) => {
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
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
