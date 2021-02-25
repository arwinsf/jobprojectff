<template>
  <div class="gif">
    <p>
      <a href="https://www.finanzfluss.de/">
        <img class="object3" src="https://media2.giphy.com/avatars/finanzfluss/Xjkrq8gHCGyt.gif" alt="hello :)"
             href="https://www.finanzfluss.de" width="120"/>
      </a>
    </p>
  </div>
</template>

<script>
export default {
  name: 'GIFComponent'
}
</script>

<style scoped>
.gif {
  position: absolute;
  height: auto;
  left: -150px;
}

.object3 {
  position: relative;
  display: block;
  overflow: hidden;
  background-size: cover;
  z-index: 1;
  -webkit-animation-name: funnyhaha;
  -webkit-animation-duration: 10s;
  animation-name: funnyhaha;
  animation-duration: 10s;
}

@keyframes funnyhaha {
  0% {
  }
  10% {
    -webkit-transform: rotate(0deg);
    left: 0
  }
  55% {
    -webkit-transform: rotate(25deg);
    left: 120px
  }
  100% {
    -webkit-transform: rotate(0deg);
    left: 0
  }
}
</style>
