<template>
  <div id="app">
    <form class="form">
      <div class="comment-header">Нашел здесь основную проблему нашей платформы</div>
      <div class="comment-list">
        <div
          class="comment-item"
          v-for="(item, index) in comments"
          :key="index"
        >
          <div class="comment-avatar"></div>
          <div class="comment-info">
            <div class="comment-name">{{ item.name }}</div>
            {{ item.comment }}
          </div>
        </div>

      </div>
      <input
        v-model="commentText"
        type="text"
        placehoder="Введи коммент"
      />
      <button @click="addCommentToList()"> Опубликовать</button>
    </form>
    <iframe
      ref="ta"
      id="iframe"
      src="https://app.matter.online"
      seamless
    ></iframe>
  </div>
</template>

<script>
export default {
  name: 'App',

  data () {
    return {
      currentUser: {
        userId: 1,
        name: 'Tigran',
        comment: null
      },
      commentText: '',
      comments: [
        {
          usedId: 2,
          name: 'Katerine',
          comment: 'Hey there! I have some problems'
        },
        {
          userId: 3,
          name: 'Katerine',
          comment: 'Hey there!'
        },
        {
          userId: 4,
          name: 'Katerine',
          comment: 'Hey'
        }
      ]
    }
  },

  methods: {
    select: event => {
      console.log(event)
    },

    addCommentToList () {
      this.comments.push({
        usedId: 5,
        name: this.currentUser.name,
        comment: this.commentText
      })
      this.commentText = '';
    }
  },

  mounted () {
    document
      .getElementById('iframe')
      .contentDocument.addEventListener('click', event => this.select(event))
  }
}
</script>

<style style="scss">
* {
  box-sizing: border-box;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

iframe {
  width: 100%;
  height: 100%;
  position: fixed;
  z-index: 5;
  top: 0;
  left: 0;
}

.form {
  background: #fff;
  padding: 16px;
  width: 100%;
  max-width: 320px;
  position: absolute;
  right: 250px;
  z-index: 999;
  text-align: left;
  border-radius: 10px;
  border: 1px solid #eee;
  box-shadow: 0 -4 -4 rgba(0, 0, 0, 0.9);
}

.comment-header {
  margin-bottom: 24px;
}

.content-list {
  overflow-y: scroll;
}

.comment-item {
  margin-bottom: 24px;
  display: flex;
}

.comment-info {
  margin-bottom: 4px;
  font-size: 16px;
  color: #000;
}

.comment-name {
  font-size: 14px;
  font-weight: 600;
}

.comment-avatar {
  margin-right: 16px;
  flex: 0 0 32px;
  width: 32px;
  height: 32px;
  background: red;
  border-radius: 100px;
}

input {
  width: 100%;
  font-size: 16px;
  color: #000;
  padding: 8px 8px 8px 8px;
}
</style>
