<script setup>
  import { VueElement, ref } from 'vue'



  const counter = ref(0)
  const flipText = ref("Flip")
  const pg = ref("")
  const a = ref("jdfksal")

  
  const url = 'https://flaskapitest.onrender.com/'; 


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
</script>

<script>
  import axios from 'axios';
  import * as THREE from 'three';
  import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
  import TextBlock from './components/TextBlock.vue'
  

  export default {
    name: 'App',
    components: {
      TextBlock
    },
    props: {

    },
    mounted () {

      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
      const renderer = new THREE.WebGLRenderer({
        canvas: this.$refs.a
      })

      window.addEventListener("resize", function () {
        camera.aspect = window.innerWidth / window.innerHeight;
        camera.updateProjectionMatrix();
        console.log(window.innerWidth)
        renderer.setSize(window.innerWidth, window.innerHeight)
      })

      camera.position.y = 100;
      camera.position.x = document.body.getBoundingClientRect().width / 100;
      camera.rotation.z = 0;

      renderer.setPixelRatio( window.devicePixelRatio );
      renderer.setSize( window.innerWidth, window.innerHeight );

      const geometry = new THREE.TorusGeometry( 10, 3, 16, 100 )
      const material = new THREE.MeshStandardMaterial( { color: 0xFF6347 } );
      const tourus = new THREE.Mesh( geometry, material );

      const pointLight = new THREE.PointLight(0xffffff, 100)
      const ambientLight = new THREE.AmbientLight(0xffffff, 1)

      const lightHelper = new THREE.PointLightHelper(pointLight)
      const gridHelper = new THREE.GridHelper(200, 50)

      const spaceTexture = new THREE.TextureLoader().load(
        "./src/assets/space.jpg"
      )
      scene.background = spaceTexture

      
      pointLight.position.set(-5,10,10)
      scene.add(pointLight, lightHelper, gridHelper)
      scene.add(tourus)
      scene.add(ambientLight)

      const controls = new OrbitControls(camera, renderer.domElement)

      function addStar() {
        const geometry = new THREE.SphereGeometry(0.25,24,24)
        const material = new THREE.MeshStandardMaterial({ color: 0xffffff})
        const star = new THREE.Mesh( geometry, material )

        const [x, y, z] = Array(3).fill().map(() => THREE.MathUtils.randFloatSpread( 100 )) 

        star.position.set(x, y, z)
        scene.add(star)

      }

      function moveCamera() {
        const t = document.body.getBoundingClientRect().top;

        tourus.rotation.x += 0.01;
        tourus.rotation.y += 0.005;
        tourus.rotation.z += 0.01;
        if (t != 0){
          camera.position.y = (t / document.body.getBoundingClientRect().height * 200) + 100;
          camera.position.x = (t * -0.002) + (document.body.getBoundingClientRect().width / 100);
          camera.rotation.z = t * 0.0002;
        }
      }

      document.body.onscroll = moveCamera
      
      Array(300).fill().forEach(addStar)

      function animate() {
        requestAnimationFrame( animate );

        

        controls.update()


        renderer.render( scene, camera );
      }

      animate()
    },
  }
</script>

<template>
  <canvas ref="a"></canvas>
  <div id="chunck">
    <h2>P:{{ pg }}</h2>
    <h3>{{ a }}</h3>
    <button @click="incrementCounter">Counter: {{ counter }}</button>
    <button @click="flip">{{ flipText }}</button>
    <button @click="getSite">Get Site</button>
    <TextBlock></TextBlock>
    <TextBlock></TextBlock>
    <TextBlock></TextBlock>
    <TextBlock></TextBlock>
    <TextBlock></TextBlock>
    <TextBlock></TextBlock>
    <TextBlock></TextBlock>
    <TextBlock></TextBlock>
    <TextBlock></TextBlock>
    <TextBlock></TextBlock>
  </div>

</template>

<style>
* {
  color: aliceblue;
  overflow-x: hidden;
}
canvas {
  background-color: aqua;
  z-index: 1;
  position: fixed;
  top:0;
  left:0;
}
::-webkit-scrollbar {
    width: 0px;
}

::-webkit-scrollbar-track {
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
}

::-webkit-scrollbar-thumb {
  background-color: darkgrey;
  outline: 1px solid slategrey;
}
#app {
  scrollbar-width: none;
  overflow-x: hidden;
  z-index: 2;
  display: flex;
  flex-direction: column;
  width: 100vw;
  align-items: center;
  justify-content: center;
  overflow-y: scroll;

}
#chunck {
  overflow: hidden;
  z-index: 2;
  display: flex;
  flex-direction: column;
  width: 100vw;
  align-items: center;
  justify-content: center;
  padding: 20%;
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
