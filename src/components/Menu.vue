<template>
<nav id="sidebarMenu" class="menu__sidebar">
      <div class="position-sticky pt-3">
      
        <ul class="menu__nav">
          <li class="menu__list_item">
            <router-link class="menu__item"  to="/" >Dashboard
            </router-link>
          </li>
          <li class="menu__list_item">
            <router-link  class="menu__item" to="/users">Users
            </router-link>
          </li>
          <li class="menu__list_item">
            <router-link  class="menu__item" to="/roles">Roles
            </router-link>
          </li>
          <li class="menu__list_item">
            <router-link  class="menu__item" to="/products">Products
            </router-link>
          </li>
          <li class="menu__list_item">
            <router-link class="menu__item" to="/orders">Orders
            </router-link>
          </li>
          <li class="menu__list_item">
            <router-link   class="menu__item" to="/cart">Cart
            </router-link>
          </li>
          <li class="menu__list_item">
            <router-link  class="menu__item" to="/orderTest">Test
            </router-link>
          </li>
          
        </ul>
    </div>
        
      
    </nav>
    
</template>

<script>
export default {
    name:'Menu'
    
}
</script>

<style scoped>

.menu__sidebar{
  padding:10px;
  position:fixed;
  width:15vw;
  top: 0;
  bottom: 0;
  left: 0;
  z-index: 100;
  background-color: #333;
  
  
}

.menu__nav{
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  list-style: none;
  margin-top: 35px;

}

.menu__item{
display: block;
padding: .5rem 1rem;
text-decoration: none;
  font-size:17px;
  font-weight: 300;
  color:white;
text-decoration: none;
perspective:1000px;
  background-image: linear-gradient(10deg ,transparent 0%, transparent 50%,  white 50% ) ;
  background-size: 440%;
  background-position: 139%;


    transition: all .6s ease-out;
}

.menu__item:hover,
.menu__item:active{
  color: #333;
      
      background-position: 100%;
      transform:  translateX(-20px) ;
    }



.menu__item--active{

}

.menu__list_item:not(:last-child) {
      margin-bottom: 15px; }


</style>