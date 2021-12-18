<template>
    
    <div class="card__wrapper"  @click="increase()">
        <div class="card__container">
        
            <img  class="card__image" :src="props.cart_item.product.image" width="100">
        </div>
        <div class="card__info">

            <div class="card__title"><b>{{props.cart_item.product.title}}</b></div>
            <div class="card__extra">
                <div class="card__description">{{props.cart_item.product.description}}</div>
                <div class="card__price">Price:${{props.cart_item.product.price}}</div>
            </div>
        </div>
        
    </div>
    <div class="card__count">
        <span class="card__hit-count">{{props.cart_item.count}}</span>
        <div class="card__erase-count" @click="reset()"></div>
    </div>
    
    
</template>

<script lang="ts">
import { ref, SetupContext, watch } from 'vue';
export default{
    name:"CartCard",
    props:{
        cart_item:Object
    },
    emits:['add', 'remove'],
    setup(props:any, context:SetupContext){

        const increase = () =>{
            
            
            context.emit('add',props.cart_item.product.id);
            
        }

        const reset = () =>{
            
            context.emit('remove',props.cart_item.product.id);

        }

        return{
            props,
            increase,
            reset

        }

    }
}
</script>

<style>
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

::selection{
  background-color: rgba(41,52,255,.85);
  color: white;

}
</style>