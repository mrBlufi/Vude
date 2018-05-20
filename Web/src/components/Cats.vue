<template>
  <stack v-infinite-scroll="response" infinite-scroll-disabled="busy" infinite-scroll-distance="1500" infinite-scroll-immediate-check="true"
   :column-min-width="230" monitor-images-loaded>
    <stack-item class="img-container" v-for="image in images" :key="image.url.tex">
      <img :src="image.url._text"/>
    </stack-item>
  </stack>
</template>
<script>
import request from 'request'
import { Stack, StackItem } from 'vue-stack-grid'

export default {
  created: function () {
    this.response()
  },
  components: {
    Stack, StackItem
  },
  data () {
    return {
      images: [],
      busy: false
    }
  },
  methods: {
    response: function () {
      this.busy = true
      request.post({url: 'http://localhost:1337/cat', json: [ {id: 1, messege: 'test'}, {id: 1, messege: '2'} ]},
        (error, response, body) => {
          if (error) {
          }
          this.images.push(...response.body.image)
        })
      this.busy = false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.img-container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: center;
  overflow: hidden;
  margin: 0;
  object-fit: fill;
  cursor: pointer;
}
.img-container img {
  display: block;
  margin: 0;
  width: 100%;
  height: auto;
}
.img-container figcaption {
  margin: 3px 0;
  text-align: center;
}
</style>
