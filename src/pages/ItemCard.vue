<template>


    <div class="buy__nav">
  
        <svg class="buy__layout"  version="1.1" viewBox="0 0 649.42 111.03" xmlns="http://www.w3.org/2000/svg">
        
        <defs>
            <linearGradient id="grad2" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:rgba(29,199,234);stop-opacity:0.85" />
            <stop offset="50%" style="stop-color:rgba(41,52,255);stop-opacity:0.85" />
            <stop offset="100%" style="stop-color:rgba(59,89,152) ;stop-opacity:0.85" />
            </linearGradient>
        </defs>
        <g transform="translate(173 -10)">
        <path d="m124.45 7.4579-5.3e-4 0.08992-292.6 0.48111s-11.565 45.681 18.277 43.979c157.69-8.9971 217.61 26.396 219.64 38.267 3.4921 20.459 57.115 22.913 57.115 22.913l24.831-0.0987 24.831 0.0987s53.623-2.4538 57.115-22.913c2.0263-11.872 61.945-47.264 219.64-38.267 29.842 1.7026 18.277-43.979 18.277-43.979l-292.6-0.48111-5.3e-4 -0.08992-27.262 0.04496z" fill= "url(#grad2)"/>
        </g>
        <foreignObject class='buy__area'>
            <div>
            <button class="buy__button" style="transform:scale(1)">BUY</button>
            </div>
        </foreignObject>
        
        
        </svg>
  
    </div>



<div class="card__section">

    <div class="card__displayer">
        <div class="card__content" v-for="prod in prods" :key="prod.id" @click="select(prod.id)" :class="selected===prod.id?'show':'hide'" v-on:animationend="removeSel()">
            <div class="card__wrapper" >
                <div class="card__container">
                
                    <img  class="card__image" :src="prod.image" width="100">
                </div>
                <div class="card__info">

                    <div class="card__title"><b>{{prod.title}}</b></div>
                    <div class="card__extra">
                        <div class="card__description">{{prod.description}}</div>
                        <div class="card__price">Price:${{prod.price}}</div>
                    </div>
                </div>
                
            </div>
            <div class="card__count">
                <span class="card__hit-count">13</span>
                <div class="card__erase-count"></div>
            </div>
        </div>
    </div>
</div>
    
</template>

<script>
import { onMounted, ref } from 'vue'
import axios from 'axios'
export default {
    name:"ItemCard",
    setup() {
        const prods = ref([]);

        const selected = ref(0)

        const select = (id)=>{

            selected.value = id;
        }

        const removeSel = () =>{
            selected.value = 0;

        }

        onMounted ( async () =>{
            const {data} = await axios.get(`product`)
            prods.value = data.data

        });

        return{
            prods,
            select,
            selected,
            removeSel
        }
        
    },
}
</script>

<style scoped>

.card__section{
    margin-top:100px;
}

.card__displayer{
    margin:5rem 3rem;
    width: 100%;
    display: grid;
  grid-template-columns: repeat(auto-fit, 17rem);
  grid-auto-rows:20rem;
  grid-auto-flow: dense;
  grid-gap: 4rem;
  

}

.card__wrapper{

    width:100%;
    height:100%;
    display:grid;
    
    
    grid-template-columns: repeat(2, 1fr);
    grid-row-gap: .5rem;
    overflow: hidden;
    backface-visibility: hidden;
    border-radius: 8px;
    box-shadow:  5px 5px 35px;
    background-image:linear-gradient( to right bottom,
                                    rgba(41,52,255,0.85),
                                    rgba(29,199,234,0.85))
    
    
}


.card__content::after{
    content:"";
    display: inline-block;
    height: 100%;
    width:100%;
    border-radius: 14px;
    background-image:linear-gradient( to right bottom,
                                    rgba(41,52,255,0.85),
                                    rgba(29,199,234,0.85));
    top:0;
    left: 0;
    position: absolute;
    z-index : -1;
    transition: all .5s ;
  }


.card__content{
    position: relative;
    width:15rem;
    height:18rem;
    perspective:1000px;
    -moz-perspective: 1000px;
    
    backface-visibility: hidden;
    transition: all .4s ease-in-out;

}

.card__content:hover{
    width: 18rem;
    height: 22rem;
    transform: scale(1.07) , rotate(0.01);
    
}

.show::after{
animation-name:ripple;
animation-duration : .4s
}


@keyframes ripple {
  0%{
    opacity: 1;
    background-color: blue;
  }

  99%{
    opacity:0;
    transform: scale(1.3);
  }

  100%{
    opacity: 1;
    transform: scale(1);
  }
}

.hide::after{
    background-image:linear-gradient( to right bottom,
                                    rgba(41,52,255,0.85),
                                    rgba(29,199,234,0.85));
      opacity: 1;
}


/* .card__content:active::after{
    transform: scale(1.3);
    background-color: blue;
      opacity: 0;
} */



.card__content:hover>.card__wrapper{
    background-image:linear-gradient( to right bottom,
    rgba(59,89,152,0.85),
                                    rgba(41,52,255,0.85)
                                    );
    
}

.card__container{
    
    grid-column: 1/-1;
    grid-row: 1/2;
    z-index: 1;
    overflow: hidden;
    max-height: 10rem;
    text-transform:uppercase;
    

}


.card__image{
    width: 100%;
    height: 100%;
    object-fit:cover;
    
}

.card__info{
    grid-column: 1/-1;
    grid-row: 2/3;
    

}


.card__title{
    color:white;
    
    justify-self: center;
    font-size: 1.1rem;
    text-align: center;
    font-weight: 400;
    text-transform:uppercase;
}

.card__extra{
    color:white;
    visibility: hidden;
    opacity: 0;
    transition: all .4s;
    padding:3px;
    width:100%;
    
    

}

.card__description{
    margin: 0 10px;
    text-align: center;
}

.card__price{
    font-size: 1.1rem;
    font-weight: 300;
    justify-self: center;
    text-align: center;
    padding: 4px 4px;
    transition: all .4s;
}

.card__content:hover>.card__wrapper> .card__info>.card__extra>.card__price{
    margin-top:1rem;
}

.card__content:hover>.card__wrapper> .card__info>.card__extra{
    visibility: visible;
    opacity: 1;
}

.card__content:hover>.card__count{
    visibility: visible;
    opacity: 1;
    
}

.card__count{
    
    transition:all .1s;
    transition-delay: .5s;
}

.card__hit-count{
    font-size: 1rem;
    height: 1.75rem;
    width: 1.75rem;
    border-radius: 100px;
    background-image:linear-gradient( to right bottom,
    rgba(46, 235, 13, 0.85),
                                    rgba(3, 161, 109, 0.85)
                                    );
    color:#fff;
    position: absolute;
    top:-1.5rem;
    right: -1.1rem;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index : 30;
    outline: 3px solid white;
     -moz-outline-radius: 100px;
    
}

.card__erase-count{
    font-size: 1rem;
    height: 1.75rem;
    width: 1.75rem;
    border-radius: 100px;
    background-image:linear-gradient( to right bottom,
    rgba(235, 13, 24, 0.85),
                                    rgba(161, 3, 29, 0.85)
                                    );
    color:#fff;
    position: absolute;
    top:1.3rem;
    right: -1.1rem;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index : 30;
    outline: 3px solid white;
     -moz-outline-radius: 100px;
}

.card__erase-count::after{
    width: 20px;
      height: 3px;
      background-color: white;
      display: inline-block;
            content: "";
      position: absolute;
      transform: rotate(135deg);

}

.card__erase-count::before{
    width: 20px;
      height: 3px;
      background-color: white;
      display: inline-block;
            content: "";
      position: absolute;
      transform: rotate(-135deg);

}


.buy__nav{
  
   z-index:1;
   transition:all .3s;
  transform:translateY(-70px);
  position:fixed;
}

.buy__nav:hover{
    transform:translateY(0);
}

.buy__layout{
  width:95%;
  z-index:1;
  
  
}

.buy__area{
  width:100%;
  height:100%;
  z-index:100;
  
  position: relative;
  
}

.buy__button{
  margin: 0;
  position: absolute;
  width:130px;
  height:40px;
  border:none;
  top: 45%;
  left: 40%;
  transform: translate(-50%, -50%);
  background-color:rgba(41,52,255,.85);
  color:white;
  border-radius: 1000px;
  transition:all .4s;
  

}

.buy__button:hover{
    background-color:rgba(59,89,152,0.85);
    outline: none;
    animation: pulsate 1s infinite;
}

@keyframes pulsate {

  0%{

    transform:  scale(1);
    box-shadow: none;

  }

  50%{

    transform:  scale(1.05);
    box-shadow: 0 5px 9px rgba(0,0,0,.2);
  }

  100%{

    transform: scale(1);
    box-shadow: none;

  }

}

</style>