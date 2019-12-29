<template>
  <div>
     <canvas
     ref="game" 
     width="700" 
     height="700" 
     style="border: 2px solid black;">
    </canvas>
    <p>
        <button v-on:click="move('right')">Right</button>
        <button v-on:click="move('left')">Left</button>
        <button v-on:click="move('up')">Up</button>
        <button v-on:click="move('down')">Down</button>
    </p>
    <p>TOUCHES : ZQSD </p>
  </div>
</template>
<script> 

</script>
<script>
  import io from "socket.io-client"
  
  export default {
    name: 'BadGame',
    data() {
      return {
        socket: {},
        context: {},
        position: {
          x: 0,
          y: 0,
        }
      }
    },
    created() {
      this.socket = io("http://localhost:3000"); //à adapter selon l'addresse du serveur
    },
    mounted() {
      window.addEventListener("keypress", e => {
      if(String.fromCharCode(e.keyCode) == "d"){this.socket.emit("move", "right");}
      if(String.fromCharCode(e.keyCode) == "q"){this.socket.emit("move", "left");}
      if(String.fromCharCode(e.keyCode) == "z"){this.socket.emit("move", "up");}
      if(String.fromCharCode(e.keyCode) == "s"){this.socket.emit("move", "down");}
    });
      this.context = this.$refs.game.getContext("2d");
      this.socket.on("position", data => {
          this.position = data;
          this.context.clearRect(0, 0, this.$refs.game.width, this.$refs.game.height);
          this.context.fillRect(this.position.x , this.position.y, 20 ,20)
      });
    },
    methods: {
      move(direction) {
        this.socket.emit("move", direction);
      }
    }
  }
</script>

<style scoped>
</style>
