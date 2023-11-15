<script setup>
   import { ref, onMounted } from 'vue';

   const canvas = ref(null);
   const video = ref(null);
   const ctx = ref(null);

   const constraints = ref({
    audio: false,
    video: true   
   });

   onMounted(async () => {
    if (video.value && canvas.value) {
      ctx.value = canvas.value.getContext("2d");

      await navigator.mediaDevices
      .getUserMedia(constraints.value)
      .then(SetStream)
      .catch(e => {
          console.error(e);
      })
    }
   });

   function SetStream(stream) {
    video.value.srcObject = stream; 
    video.value.play();

    requestAnimationFrame(Draw);
   }

   function Draw() {
    // ctx.value.clearRect(0, 0, canvas.value.width, canvas.value.height);
    ctx.value.drawImage(video.value, 0, 0, canvas.value.width, canvas.value.height);
    requestAnimationFrame(Draw);
   }


   function TakePicture() {
      var link = document.createElement("a"); 
      link.download = `vue-cam-${new Date().getTime().toISOString()}.png`;
      link.href = canvas.value.toDataURL("image/png");
      link.click();
   }
</script>

<template>
    <div class="text-center leading-8 antialiased font-normal">
      <h3>» Hook up a webcam to take a snapshot of yourself «</h3>
      <video ref="video" autoplay playsinLine webkit-playsinLine muted hidden></video>

      <canvas ref="canvas" width="1280" height="720" class="bg-black rounded"></canvas>

      <div class="flex items-center justify-center py-4">
        <button 
          @click="TakePicture" 
            class="
            px-8 py-4 bg-hotpink-500 rounded
            text-white text-2xl uppercase font-bold
            hover: bg-red-600
            ">
            Take snapshot !
        </button>

      </div>



    </div>
</template>

<style>

body 
{
  background: ghostwhite;
    /* background: linear-gradient(45deg, #888, #666, #888); */
}

</style>
