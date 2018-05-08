<template>
  <div>
    <div v-infinite-scroll="response" infinite-scroll-disabled="busy" infinite-scroll-distance="1500" infinite-scroll-immediate-check="false">
      <div >
       <img v-for="image in images" :key="image.url.tex" v-bind:src="image.url._text"/>
    </div>
  </div>

  </div>
</template>
<script>
import request from 'request'

export default {
  created: function () {
    this.response()
  },
  data () {
    return {
      images: [],
      busy: false
    }
  },
  methods: {
    response: function () {
      console.log('start')
      this.busy = true
      request.post({url: 'http://localhost:1337/cat', json: [ {id: 1, messege: 'test'}, {id: 1, messege: '2'} ]},
        (error, response, body) => {
          if (error) {
            console.log(error)
          }
          this.images.push(...response.body.image)
        })
      this.busy = false
      console.log('end')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
img{
  width: 300px;
  height: 300px;
}
</style>
