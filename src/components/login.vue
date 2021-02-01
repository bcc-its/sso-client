<template>
   <button type="button" 
                                @click="implicit">
                                Implicit Flow - For Browsers
  </button>
  <h3>id_token</h3>
  <vue-json-pretty :path="'res'" :data="id_token" @click="handleClick"> </vue-json-pretty>
<h3>id_token encoded: {{id_token_encoded}}</h3>
  
  <h3>access_token: {{access_token}}</h3>
  <h3>code: {{code}}</h3>
  
  

</template>

<script lang="ts">
import { ref, defineComponent } from 'vue'
import  queryString from 'query-string'
import jwt_decode from "jwt-decode";
import VueJsonPretty from 'vue-json-pretty';
import 'vue-json-pretty/lib/styles.css';

export default defineComponent({
  name: 'HelloWorld',
  components:{
    VueJsonPretty
  },
  props: {
    msg: {
      type: String,
      required: true
    }
  },
  setup: () => {
    const count = ref(0)    
    return { count }
  },
  data:()=>{
    return {
      id_token:{},
      id_token_encoded:'',
      access_token:'',
      code:''
    }
  },
  created: function(){
    const parsed = queryString.parse(location.hash);
    this.id_token = jwt_decode(parsed.id_token)
    this.id_token_encoded = parsed.id_token
    this.access_token = parsed.access_token
    this.code = parsed.code
    console.log(parsed)
    
    
  },
  methods:{
    implicit: ()=>{
      const url = 'https://devlogin.bcc.no/authorize'
      const response_type = 'code id_token token'
      const redirect_uri = 'http://localhost:3000/'
      const scope = 'openid profile church country phone email'      
      const client_id = 'P4nlyZuTGGHLlluUUauJy5yehM9xZrvK'
      const nonce = 'n-0S6_WzA2Mj'
      const state = 'af0ifjsldkj'

      const request = `${url}?response_type=${response_type}
&client_id=${client_id}
&redirect_uri=${redirect_uri}
&scope=${scope}
&nonce=${nonce}
&state=${state}`

      console.log(request)
      window.location = request;
    }
  }
})
</script>

<style scoped>
a {
  color: #42b983;
}
</style>