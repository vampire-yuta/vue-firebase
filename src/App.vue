<template>
  <div>
    <div id="app">
      <h3>掲示板に投稿する</h3>
      <label for="name">ニックネーム：</label>
      <!-- <input id="name" type="text"> -->
      <input v-model="name" type="text">
      <p>{{ name }}</p>
      <br><br>
      <label for="comment">コメント：</label>
      <textarea v-model="comment"></textarea>
      <button @click="createComment">送信</button>
      <h2>掲示板</h2>
    </div>
    <div v-for="post in posts" :key="post.name">
      <br>
      <div>名前:{{ post.fields.name.stringValue }}</div>
      <div>コメント:{{ post.fields.comment.stringValue }}</div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
axios.defaults.baseURL = 'https://firestore.googleapis.com/v1/projects/xxxxxxxxxxx/databases/(default)/documents/comments'
export default {
  data() {
    return {
      name: "",
      comment: ""
    }
  },
  created() {
    axios.get(
      axios.defaults.baseURL)
      .then(response => {
        console.log(response.data)
        this.posts = response.data.documents;
      });
  },
  methods: {
    createComment() {
      axios.post(
        axios.defaults.baseURL,
        {
          fields: {
            name: {
              stringValue: this.name
            },
            comment: {
              stringValue: this.comment
            }
          }
        }
      )
        .then(response => {
          console.log(response)
        })
        .catch(error => {
          console.log(error)
        });
      this.name = ""
      this.comment = ""
    },
  }
}
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
