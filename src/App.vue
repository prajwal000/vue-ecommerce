
<template>

<Navbar/>
  <Home/>
</template>
<script>
import Navbar from './Components/Navbar.vue'
import Home from './pages/Home.vue';

export default {
  components: {
    Navbar,
    
    Home
}
}
</script>