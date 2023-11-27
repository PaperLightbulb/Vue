<script setup>
  import { VueElement, ref } from 'vue'


  const counter = ref(0)
  const flipText = ref("Flip")
  const pg = ref("")
  const a = ref("jdfksal")




  function incrementCounter () {
    counter.value++
  }
  function flip () {
    flipText.value = flipText.value.split("").reverse().join("")
    setA(a.value + "a")
  }
  function setA (str) {
    a.value = str
  }
  function getSite () {
    axios.get(url).then((request) => {
      setA("The Value is " 
      + request.data['temp']
    )}).catch((error) => {
      console.log(error)
    })
  }
  function post () {
    axios.post(url+"/p/", counter.value)
    .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
  }
</script>

<script>
  import axios from 'axios'

  const url = 'https://flaskapitest.onrender.com/'; 

  

  export default {
    name: 'App'
  }
</script>

<template>
  <div>
    <h2>P:{{ pg }}</h2>
    <h3>{{ a }}</h3>
    <button @click="incrementCounter">Counter: {{ counter }}</button>
    <button @click="flip">{{ flipText }}</button>
    <button @click="getSite">Get Site</button>
    <button @click="post">Post</button>
  </div>
</template>

<style>
div {
  display: flex;
  flex-direction: column;
  width: 100vw;
  height: 100vh;
  align-items: center;
  justify-content: center;
}
button {
  border: 0px;
  background-color: black;
  color: white;
  border-radius: 50vh;
  margin: .5%;
  font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  font-size: 14pt;
}
</style>
