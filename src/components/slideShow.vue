<template>
    <h2 class="title">
        <span 
            v-for="symbol in text" 
            :key="symbol"
            class="symbol"
        >
            {{symbol}}
        </span>
    </h2>
    <div class="price symbol">{{img.price}}</div>
    <button class="btn">
        Buy now
        <span></span>
        <span></span>
        <span></span>
        <span></span>
    </button>
    <img :src="require('@/assets/'+img.url)" :alt="img.name" class="image">

</template>

<script>
    export default {
        data() {
            return {
                text: []
            }
        },
        props:{
            img:{
                type:Object,
                require:true
            }
        },
        mounted(){
            this.text = this.img.name.trim().split('');
        }
    }
</script>

<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@500&display=swap');
*{
    font-family: 'Noto Sans JP', sans-serif;
}
.image{
    height: 60vh;

    @media screen and (max-width:500px){
        &{
            height: 40vh;
        }
    }
}
.title{
    margin-bottom: 2rem;
    color:#FF6495;
    letter-spacing: 5px;
    font-size: 4rem;
    cursor: context-menu;

    @media screen and (max-width:777px){
        &{
            font-size:2rem;
        }
    }
    @media screen and (max-width:500px){
        &{
            font-size:1.5rem;
        }
    }
    @media screen and (max-width:350px){
        &{
            font-size:1rem;
        }
    }
}
.symbol{
    display: inline-block;
    letter-spacing: 5px;
}
.symbol:hover{
    animation-duration: 1s;
    animation-fill-mode: both;
    animation-iteration-count: 1;
    animation-name: special;
    color:#361999;
}

@keyframes special {
    from {
    transform: scale3d(1, 1, 1);
  }

  30% {
    transform: scale3d(1.25, 0.75, 1);
  }

  40% {
    transform: scale3d(0.75, 1.25, 1);
  }

  50% {
    transform: scale3d(1.15, 0.85, 1);
  }

  65% {
    transform: scale3d(.95, 1.05, 1);
  }

  75% {
    transform: scale3d(1.05, .95, 1);
  }

  to {
    transform: scale3d(1, 1, 1);
  }
}

.price {
    font-size: 2rem;
    color: #1A2238;
    cursor:context-menu;
}

.btn {
    margin: 0;
    padding: 0;
    position:absolute;
    bottom:9%;
    right: 5%;
    --c: #371BB1;
    color: var(--c);
    font-size: 16px;
    border: none;
    outline:none;
    border-radius: 0.5em;
    width: 12em;
    height: 3em;
    text-transform: uppercase;
    font-weight: bold;
    font-family: sans-serif;
    letter-spacing: 0.1em;
    text-align: center;
    line-height: 3em;
    overflow: hidden;
    z-index: 1;
    transition: 0.5s;
    margin: 1em;
}

.btn span {
    position: absolute;
    width: 25%;
    height: 100%;
    background-color: var(--c);
    transform: translateY(150%);
    border-radius: 50%;
    left: calc((var(--n) - 1) * 25%);
    transition: 0.5s;
    transition-delay: calc((var(--n) - 1) * 0.1s);
    z-index: -1;
}

.btn:hover {
    color: black;
}

.btn:hover span {
    transform: translateY(0) scale(2);
}

.btn span:nth-child(1) {
    --n: 1;
}

.btn span:nth-child(2) {
    --n: 2;
}

.btn span:nth-child(3) {
    --n: 3;
}

.btn span:nth-child(4) {
    --n: 4;
}

</style>