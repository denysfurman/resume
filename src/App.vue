<template>
  <div class="container column">
    <form @submit.prevent class="card card-w30">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select v-model="form.type" id="type">
          <option value="title">Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="text">Текст</option>
        </select>
      </div>
      <div class="form-control">
        <label for="value">Значение</label>
        <textarea v-model="form.text" id="value" rows="3"></textarea>
      </div>
      <button :disabled="!validationButton"  @click="createBlock" class="btn primary">Добавить</button>
    </form>

    <div class="card card-w70">
      <template v-if="blocks.length">
        <div v-for="(block, idx) in blocks" :key="idx">
          <Title v-if="block.type === 'title'" :title="block.text"/>
          <Avatar v-else-if="block.type === 'avatar'"  :url="block.text"/>
          <SubTitle v-else-if="block.type === 'subtitle'"  :title="block.text"/>
          <Text v-else-if="block.type === 'text'"  :text="block.text"/>
        </div>
      </template>
      <h3 v-else >Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
  </div>
  <div class="container">
    <p v-if="!showComments">
      <button @click="loadComments" class="btn primary">Загрузить комментарии</button>
    </p>
    <div v-if="showComments" class="card">
      <h2>Комментарии</h2>
      <ul class="list">
        <li v-for="comment in comments" :key="comment.id" class="list-item">
          <div>
            <p><strong>{{comment.email}}</strong></p>
            <small>{{comment.body}}</small>
          </div>
        </li>
      </ul>
    </div>
    <div v-if="loading" class="loader"></div>
  </div>
</template>

<script>
import axios from 'axios'
import Title from './components/Title'
import SubTitle from './components/SubTitle'
import Text from './components/Text'
import Avatar from './components/Avatar'
export default {
  data () {
    return {
      blocks: [],
      loading: false,
      comments: [],
      showComments: false,
      form: {
        type: 'title',
        text: '',
      }
    }
  },
  components: {
    Title, Avatar, SubTitle, Text,
  },
  computed: {
    validationButton(){
      return this.form.text.length > 3;
    }
  },
  methods: {
    createBlock(){
      this.blocks.push(this.form)
      this.form =  {
        type: 'title',
        text: '',
      }
    },
    async loadComments(){
      this.loading = true;
      try {
        const { data } = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42')
        this.comments = data;
        this.showComments = true
      } catch (e) {
        alert(e.message)
      } finally {
        this.loading = false;
      }



    },
  },
}
</script>

<style>
.avatar {
  display: flex;
  justify-content: center;
}

.avatar img {
  width: 150px;
  height: auto;
  border-radius: 50%;
}
</style>
