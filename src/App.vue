<template>

  <div id="app">
    <Swagger 
      url="https://petstore.swagger.io/v2/swagger.json"
      urlBase="https://petstore.swagger.io/"
      noModel
      noTitle
      >
        <Api tag="pet"/>
        <Api operationId="loginUser"/>
    </Swagger>
  </div>
</template>

<script>
import Swagger from './components/Swagger.vue'
import Api from './components/Api.vue'
export default {
  name: 'App',
  components: {
    Swagger,
    Api
  }
}
</script>

