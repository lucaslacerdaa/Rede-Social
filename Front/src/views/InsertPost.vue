
<template>
  
  <div id="container">
    <h2>Seu Post:</h2>
    <input type="text" class="inputs_form" id="textInput" v-model="text" placeholder="O que anda pensando?"/> <br/><br/>
    <input type="text" class="inputs_form" id="2" v-model="date" placeholder="Data"><br/><br/>
    <input type="text" class="inputs_form"  id="3" v-model="time" placeholder="Horário"/><br/><br/>
    Imagem: <input type="file" id="file" ref="file" name="image" /> <br /><br />

    <button @click="inserirPosts" id="buttonPostar">Postar</button> <br /><br />

  </div>

</template>

<script>
import axios from "axios";

export default {
  name: "InsertPost",
  data() {
    return {
      id: "",
      userId: "",
      text: "",
      date: "",
      time: "",
      posts: [],
      post: "",
      baseURI: "http://localhost:3000/posts",
      baseUpload: "http://localhost:3000/upload",
    };
  },
  methods: {
    handleFileUpload(id) {
      this.file = this.$refs.file.files[0];
 
      let obj = {
        resource: "posts",
        id: id,
      };
      let json = JSON.stringify(obj);
 
      let form = new FormData();
      form.append("obj", json);
      form.append("file", this.file);
 
      console.log(form.getAll("file"));
 
      axios
        .post(this.baseUpload, form, {
          headers: {
            "Content-Type": "multipart/form-data",
          }, 
        })
        .then((result) => {
          console.log(result);
        });
    },
    inserirPosts: function() {
      axios
        .post(this.baseURI, {
          text: this.text,
          date: this.date,
          time: this.time,
        })
        .then((result) => {
          if (result.status == 201) {
            alert("Inserido com sucesso !!");
            this.handleFileUpload(result.data.id);
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
  },
  created() {
    if (localStorage.getItem("user")) {
      console.log("OK");
    } else {
      this.$router.push("/");
    }
  },
};
</script>

<style>
h2{
  background-color: transparent;
  margin-top: 0em;
  padding: 4%;
}
#buttonPostar{
  background-color: rgb(156, 255, 117);
  transition: background-color 0.2s;
}
#buttonPostar:hover{
  transition: background-color 0.2s;
  transition: color 0.2s;
  background-color: rgb(122, 255, 70);
}
.input_form{
  width: 100px;
}
</style>
