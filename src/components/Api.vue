<template>
<div>
  <slot/>
</div>
</template>

<script>
import Swagger from './Swagger'
export default {
    props:{
       tag:{
         type:String,
         default:''
         },
       operationId:{
         type:String,
         default:''
         },
    },
    require:{
      Swagger
    }
}
</script>
