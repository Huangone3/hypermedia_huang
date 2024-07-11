<script setup lang="ts">
import BreadComponent from './components/BreadComponent.vue';

</script>
<template>
  <NuxtLayout>

  <ul>
    <li class="margin"><button class="btn" onclick="history.back()"><img src="~/assets/img/backarrow.svg"></button></li>
    <li><BreadComponent></BreadComponent></li>
    <li style="float:right"><button class="butt" onclick="alert('Better Future instagram...')"><img src="~/assets/img/insta_logo.png"></button></li>
    <li style="float:right"><button class="butt" onclick="alert('Better Future telegram...')"><img src="~/assets/img/telegram_logo.png"></button></li>
    <li style="float:right"><button class="butt" onclick="alert('Better Future X...')"><img src="~/assets/img/x_logo.avif"></button></li>
  </ul>
    <NuxtPage></NuxtPage>
  </NuxtLayout>
</template>

<style scoped>

img{

}

ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
}

li {
  float: left;
}

li a {
  display: block;
  color: white;
  text-align: center;
  padding: 16px;
  text-decoration: none;
}

.btn {
  background-image: linear-gradient(to bottom, white);
  border-radius: 28px;
  color: #ffffff;
  width: 80px;
}

.btn:hover {
  background: white;
  background-image: linear-gradient(to bottom, rgba(197, 177, 177, 0.826));
  text-decoration: none;
}

img {
height: 70px;
}

.margin{
  margin-left: 10px;
  margin-right: 10px;
}
</style>