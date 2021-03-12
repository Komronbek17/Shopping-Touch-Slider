<template>
    <div class="slide-container" ref="slideContainer">
      <div class="slide"
        :class="{'grabbing':isGrabbing}" 
        v-for="(slide,index) in slides"
        :key="index"
        
          @mousedown="mouseDown"
          @mousemove="mouseMove"
          @mouseup="mouseUp"

          @touchstart="mouseDown"
          @touchmove="mouseMove"
          @touchend="mouseUp"
      >
        <slide-show :img="slide">
        </slide-show>
      </div>
    </div>
</template>

<script>
import { onMounted, ref } from 'vue';
import slideShow from './components/slideShow.vue';


export default {
  name: 'App',
  /* SETUP */
  setup(){
    let currentPos = 0;
    let startPos = 0;
    let currentTranslate = 0;
    let currentIndex = 0;

    /* make contextmenu default */
    window.addEventListener('contextmenu',(e) => e.preventDefault());
    const slideContainer=ref('');
    let isGrabbing=ref(false);

    let slides;

    onMounted(() => {
      slides=Array.from(document.querySelectorAll('.slide'));
      slides.forEach((slide) => {
        slide.addEventListener('dragstart',(e) => e.preventDefault());
      });
    })

    function positionX(event){
      return event.type.includes('mouse') ? event.clientX : event.touches[0].clientX;
    }

    /* Mouse event */
    function mouseDown(event){
      isGrabbing.value=true;
      startPos=positionX(event);
    }

    function mouseMove(event){
      currentPos=positionX(event);
    }

    function mouseUp(){
      currentTranslate=currentPos-startPos;
      isGrabbing.value=false;
      if( currentTranslate < -100 && currentIndex < slides.length-1 ){
        currentIndex++;
        moveNextSlide();
      }

      if( currentTranslate > 100 && currentIndex > 0){
        currentIndex--;
        moveNextSlide();
      }
    }

    function moveNextSlide(){
      let moveTo=currentIndex*-window.innerWidth;
      slideContainer.value.style.transform=`translateX(${moveTo}px)`;
    }

    return {
      slideContainer,
      mouseDown,
      mouseMove,
      mouseUp,
      isGrabbing
    }
  },
  /* COMPONENTS */
  components: {
    slideShow
  },
  /* DATA */
  data() {
    return {
      slides: [
          {
            url:'iphone.png',
            name:'iPhone 11',
            price:'$797'
          },
          {
            url:'redmi.png',
            name:'Redmi Note 9',
            price:'$717'
          },
          {
            url:'samsungwatch.png',
            name:'Samsung Watch',
            price:'$432'
          },
          {
            url:'samsungone.png',
            name:'Samsung Galaxy S20',
            price:'$696'
          }
        ]
    }
  },
}
</script>

<style lang="scss">
*{
  margin:0;
  padding:0;
  box-sizing: border-box;
}
body,html{
  height: 100%;
  width: 100%;
  background: #F4DB7D;
  overflow: hidden;

}
.slide-container{
  height:100vh;
  display: inline-flex;
  overflow:hidden;
  transform:translateX(0);
  transition:transform 0.7s ease-in-out;
}

.slide{
  width:100vw;
  height:100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
}
.slide img{
  cursor:grab;
}
.grabbing img{
  cursor:grabbing;
  transition:transform 0.5s ease-in-out;
  transform: scale(0.9);
}
</style>
