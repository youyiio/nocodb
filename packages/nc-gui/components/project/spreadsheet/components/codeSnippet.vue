<template>
  <div @click="invoke">
    start job
  </div>
</template>

<script>
import io from 'socket.io-client'

export default {
  name: 'CodeSnippet',
  props: {},
  data: () => ({
    socket: null
  }),
  created() {
    this.socket = io('http://localhost:9000')
    this.socket.on('connect_error', () => {
      this.socket.disconnect()
      this.socket = null
    })

    const socket = this.socket
    socket.on('connect', function(data) {
      console.log(socket.id)
      console.log('socket connected', data)
    })

    socket.on('data', (d) => {
      console.log('data', d)
      alert('completed')
    })
  },
  methods: {
    invoke() {
      this.$axios.$post('http://localhost:8080/api/v1/db/meta/job?id=' + this.socket.id)
    }
  }
}
</script>

<style scoped>

</style>
