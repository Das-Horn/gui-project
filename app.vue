<style>
  .cont {
    position:         absolute;
    left:             0;
    top:              0;
    width:            100vw;
    height:           100vh;
    display:          flex;
    flex-direction:   column;
    align-items:      center;
    flex-wrap:        nowrap;
    justify-content:  center;
    background:       linear-gradient(45deg, #C1FFFF 0%, #FFE6A5 100%);
  }
</style>

<template>
  <Head>
    <title>Vue Weather</title>
  </Head>
  <div class="cont">
    <WeatherApp />
  </div>
</template>
