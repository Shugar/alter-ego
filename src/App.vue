<template>
  <div id="app">
    <div class="iframe-container">
      <form
        class="form"
        :id="'formNumber' + item.x + '' + item.y"
        v-for="(item, index) in forms"
        :ref="'formNumber' + item.x + '' + item.y"
        :key="index"
        @mousedown="(e) => mainEH(e, ('formNumber' + item.x + '' + item.y))"
        :style="{ left: item.x + 'px', top: item.y + 'px'}"
      >
        <!-- <div class="comment-header">
          <div class="comment-avatar"></div>
          <div class="comment-info">
            <div class="comment-name">{{ item.userData.name }}</div>
            {{ item.userData.comment }}
          </div>
        </div> -->
        <div
          class="form-close"
          @click="forms.splice(index, 1)"
        >x</div>
        <div
          class="comment-list"
          :key="index"
        >
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
      <div
        @click="event => this.createForm(event)"
        id="iframe-click-area"
        ref="clickArea"
        class="iframe-click-area"
      >
      </div>
      <iframe
        ref="ta"
        id="iframe"
        src="https://app.matter.online"
        seamless
      ></iframe>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',

  data () {
    return {
      isMouseDown: false,
      amountCardId: [],
      elementPosition: [0, 0],
      mousePosition: [0, 0],
      proccessingFormId: '',
      forms: [],
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
        }
      ]
    }
  },

  methods: {
    mainEH (event, name) {
      this.mousePosition = [event.clientX, event.clientY]
      this.proccessingFormId = name
      this.elementPosition = [
        this.forms.find(x => x.name === name).x,
        this.forms.find(x => x.name === name).y
      ]
      document.addEventListener('mousemove', this.calcEH)
      document.addEventListener('mouseup', this.removeHandlers)
      document.addEventListener('contextmenu', this.removeHandlers)
    },

    calcEH (event) {
      var vector = [
        -this.mousePosition[0] + event.clientX,
        -this.mousePosition[1] + event.clientY
      ]

      this.mousePosition = [
        this.mousePosition[0] + vector[0],
        this.mousePosition[1] + vector[1]
      ]
      this.elementPosition = [
        this.elementPosition[0] + vector[0],
        this.elementPosition[1] + vector[1]
      ]

      this.updatePosition()
    },

    removeHandlers () {
      document.removeEventListener('mousemove', this.calcEH)
      document.removeEventListener('mouseup', this.removeHandlers)
      document.removeEventListener('contextmenu', this.removeHandlers)
    },

    updatePosition () {
      console.log()
      Object.keys(this.$refs).map(e => {
        if (e === this.proccessingFormId) {
          this.$refs[this.proccessingFormId][0].style.left =
            this.elementPosition[0] + 'px';
          this.$refs[this.proccessingFormId][0].style.top =
            this.elementPosition[1] + 'px';
        }
      })
    },

    createForm (event) {
      const x = event.clientX
      const y = event.clientY

      this.amountCardId = [x, y]
      this.forms.push({
        name: 'formNumber' + x + '' + y,
        x,
        y,
        userData: {
          ...this.currentUser
        }
      })
    },

    // handleMoveForm (event, item) {
    //   if (this.isMouseDown) {
    //     if (event.pageX > item.x) {
    //       item.x = item.x + (event.pageX - item.x)
    //       item.y = item.y + (event.pageY - item.y)
    //     }
    //   }
    // },

    // handleMouseDown (item, index) {
    //   this.isMouseDown = true
    // },

    // handleMouseUp (item, index) {
    //   this.isMouseDown = false
    // },

    addCommentToList (x, y) {
      this.comments.push({
        x: x,
        y: y,
        usedId: 5,
        name: this.currentUser.name,
        comment: this.commentText
      })
      this.commentText = null
      this.isMouseDown = false
    }
  },

  mounted () {}
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
  z-index: -1;
  top: 0;
  left: 0;
}

.form {
  background: #fff;
  padding: 16px;
  width: 100%;
  max-width: 320px;
  position: absolute;
  z-index: 2525;
  text-align: left;
  border: 1px solid #eee;
  box-shadow: 0 -4 -4 rgba(0, 0, 0, 0.9);
}

.comment-header {
  margin-bottom: 24px;
  display: flex;
}

.content-list {
  overflow-y: scroll;
}

.comment-item {
  margin-bottom: 24px;
  display: flex;
}

.form-close {
  content: "x";
  font-weight: bold;
  font-family: "Nunito", sans-serif;
  font-size: 24px;
  height: 24px;
  width: 24px;
  position: absolute;
  top: 0;
  right: 0;
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

.iframe-container {
  height: 100%;
  width: 100%;
  position: absolute;
  z-index: 200;
  left: 0;
  top: 0;
}

.iframe-click-area {
  width: 100%;
  height: 100%;
  opacity: 1;
  background: transparent;
  top: 0;
  left: 0;
}

input {
  width: 100%;
  font-size: 16px;
  color: #000;
  padding: 8px 8px 8px 8px;
}
</style>
