<template>

  <section class="src-components-formulario">
    <div class="jumbotron">
    <h2>Lista de tareas</h2>
    
    <form novalidate autocomplete="off" @submit.prevent="enviar()">
      
      <div class="form-group">
        <label for="desc">Descripcion</label>
        <input 
          type="text" 
          id="desc" 
          class="form-control"
          v-model="$v.f.desc.$model"
          >
          <div v-if="$v.f.desc.$error && $v.f.desc.$dirty" class="alert alert-danger mt-1">
            <div v-if="$v.f.desc.required.$invalid">Campo requerido</div>
            <div v-else-if="$v.f.desc.minLength.$invalid">La descripcion tiene que ser de mas de 10 caracteres</div>
            <div v-else-if="$v.f.desc.maxLength.$invalid">La descripcion tiene que ser maximo de 50 caracteres</div>
          </div>
      </div>


      <div class="form-group">
        <label for="nombre">Nombre</label>
        <input 
          type="text" 
          id="nombre" 
          class="form-control"
          v-model="$v.f.nombre.$model"
          >
          <div v-if="$v.f.nombre.$error && $v.f.nombre.$dirty" class="alert alert-danger mt-1">
            <div v-if="$v.f.nombre.required.$invalid">Nombre no valido</div>
            <div v-else-if="$v.f.nombre.minLength.$invalid">El nombre debe tener al menos 5 caracteres</div>
              
              
          </div>
      </div>

      
      <div class="form-group">
        <label for="mail">Email</label>
        <input 
          type="text" 
          id="email" 
          class="form-control"
          v-model="$v.f.email.$model"
          >
          <div v-if="$v.f.email.$error && $v.f.email.$dirty" class="alert alert-danger mt-1">
            <div v-if="$v.f.email.required.$invalid">Este campo es requerido</div>
            <div v-else-if="$v.f.email.email.$invalid">Este mail no es valido</div>
          </div>
      </div>



      <div class="form-group">
        <input 
          type="submit"
          :disabled="$v.$invalid"
          class="btn btn-danger mt-4"
          value="Enviar"
        >
        
      </div>

    </form>
    
    

    </div>


  </section>

</template>

<script>
  import {email,required ,minLength,maxLength} from '@vuelidate/validators'
  import { reactive } from 'vue' 
  import { useVuelidate } from '@vuelidate/core'
  export default  {
    name: 'src-components-formulario',
    props: [],
    mounted () { 
    },

     setup () {
      const f = reactive({
        desc : '',
        nombre : '',
        email : '',
      })

      const rules = {
        f: {
          desc:{
          required,
          minLength: minLength(10),
          maxLength: maxLength(50)
        },
          nombre:{
          required,
          minLength: minLength(5)
          },
          email:{
          required,
          email
          }
      } 
    }
      const v = useVuelidate(rules, { f })
 
      return { f,v }
    },

    data () {
      return {
        url:'https://5faa6410b5c645001602a795.mockapi.io/tp7'
      }
    },
    validations : {
      f: {
        desc:{
          required,
          minLength: minLength(10),
          maxLength: maxLength(50)
        },
        nombre:{
          required,
          minLength: minLength(5)
          },
        email:{
          required,
          email
          }
      } 
    },
    methods: {
      /*Envio de datos al backend*/
      async sendDatosFormAxios(datos) {
            try {
              let res = await this.axios.post(this.url, datos, {'content-type': 'application/json'})
              console.log(res.data)
            }
            catch(error) {
              console.log('HTTP POST ERROR', error)
            }
        },
     
       async enviar() {
            this.v.$touch()
            if(!this.v.$invalid) {
              let form = this.f
              //console.log(form)
              await this.sendDatosFormAxios(form)
              this.resetForm()
              this.v.$reset()
            }
        },

           resetForm() {
            this.v.f.desc.$model = ''
            this.v.f.nombre.$model = ''
            this.v.f.email.$model = ''
        }
    },

    computed: {  
    },
}

</script>

<style scoped lang="css">
  .src-components-formulario {

  }
  .jumbotron{
  /* Permalink - use to edit and share this gradient: https://colorzilla.com/gradient-editor/#1e5799+20,2989d8+50,1e5799+80&0+0,0.8+15,1+19,1+81,0.8+85,0+100;Blue+Two+Sided+Transparent */
background: -moz-linear-gradient(top,  rgba(30,87,153,0) 0%, rgba(30,87,153,0.8) 15%, rgba(30,87,153,1) 19%, rgba(30,87,153,1) 20%, rgba(41,137,216,1) 50%, rgba(30,87,153,1) 80%, rgba(30,87,153,1) 81%, rgba(30,87,153,0.8) 85%, rgba(30,87,153,0) 100%); /* FF3.6-15 */
background: -webkit-linear-gradient(top,  rgba(30,87,153,0) 0%,rgba(30,87,153,0.8) 15%,rgba(30,87,153,1) 19%,rgba(30,87,153,1) 20%,rgba(41,137,216,1) 50%,rgba(30,87,153,1) 80%,rgba(30,87,153,1) 81%,rgba(30,87,153,0.8) 85%,rgba(30,87,153,0) 100%); /* Chrome10-25,Safari5.1-6 */
background: linear-gradient(to bottom,  rgba(30,87,153,0) 0%,rgba(30,87,153,0.8) 15%,rgba(30,87,153,1) 19%,rgba(30,87,153,1) 20%,rgba(41,137,216,1) 50%,rgba(30,87,153,1) 80%,rgba(30,87,153,1) 81%,rgba(30,87,153,0.8) 85%,rgba(30,87,153,0) 100%); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#001e5799', endColorstr='#001e5799',GradientType=0 ); /* IE6-9 */
color:rgb(255, 255, 255)

  }
  hr{
    background-color: black;
  }
  pre {
    color:white
  }
  btn {
    background-color: black;
  }
</style>
