<script>
import useVuelidate from '@vuelidate/core'
import { required, email, sameAs, helpers } from '@vuelidate/validators'

export default {
  
  setup () {
    return { v$: useVuelidate() }
  },

  data(){
    return{

        name: '',
        email: '',
        password: '',
        confirmPassword: '',

        editingForm: false,

      //form class vars
      loginDivClasses: 'login-div',
      loginButtonClasses: 'button-form',
      loginInputClasses: '',
      
    }
  },

  validations(){
    return{
        name: {
          required: helpers.withMessage('Campo obligatorio', required),
        },

        email: {
          required: helpers.withMessage('Campo obligatorio', required), 
          email: helpers.withMessage('Email Inválido', email), 
          },

        password: {
          required: helpers.withMessage('Campo obligatorio', required),
          },

        confirmPassword: {
          sameAsPassword: helpers.withMessage('Las contraseñas digitadas no son iguales', sameAs(this.password)),

          },
    }
  },

  watch:{
    /*name: function(){
      this.validInputCheck('name');
      console.log("test1");
    }*/
  },

  computed: {

    buttonFormText(){ 
        return this.editingForm ? "Guardar" : "Editar";
      },

    validNameInput(){

      if(this.editingForm){

        this.loginInputClasses = ' input-form'

        if(!this.v$.name.$dirty){
          return this.loginInputClasses;
        } 

        else if(this.v$.name.$error){
          return this.loginInputClasses = ' input-form' + ' invalid-input-class';
        }

        else if(!this.v$.name.$invalid){
          return this.loginInputClasses = ' input-form' + ' valid-input-class';
        }
      } 
      else {
        return this.loginInputClasses = ' input-form-readonly'
      }

    },

    validEmailInput(){

      if(this.editingForm){

        this.loginInputClasses = ' input-form'

        if(!this.v$.email.$dirty){
          return this.loginInputClasses;
        } 

        else if(this.v$.email.$error){
          return this.loginInputClasses = ' input-form' + ' invalid-input-class';
        }

        else if(!this.v$.email.$invalid){
          return this.loginInputClasses = ' input-form' + ' valid-input-class';
        }
      }
      else {
        return this.loginInputClasses = ' input-form-readonly'
      }
    },

    validPasswordInput(){

      if(this.editingForm){

        this.loginInputClasses = ' input-form'

        if(!this.v$.password.$dirty){
          return this.loginInputClasses;
        } 

        else if(this.v$.password.$error){
          return this.loginInputClasses = ' input-form' + ' invalid-input-class';
        }

        else if(!this.v$.password.$invalid){
          return this.loginInputClasses = ' input-form' + ' valid-input-class';
        }
      }
      else {
        return this.loginInputClasses = ' input-form-readonly'
      }
    },

    validSamePasswordInput(){

      if(this.editingForm){

        this.loginInputClasses = ' input-form'

        if(!this.v$.confirmPassword.$dirty || this.v$.password.$error){
          return this.loginInputClasses;
        } 

        else if(this.v$.confirmPassword.$error){
          return this.loginInputClasses + ' invalid-input-class';
        }

        else if(!this.v$.confirmPassword.$invalid){
          return this.loginInputClasses + ' valid-input-class';
        }
      }
      else {
        return this.loginInputClasses = ' input-form-readonly'
      }
    },

  },

  methods: {

    formMode(){

      if(!this.editingForm){
        this.editingForm = !this.editingForm;
      }
      else {
        this.submit();
      }

    },


    async submit(){
      const result = await this.v$.$validate();

      if (!result){
        console.log("formulario invalido");
      }
      else{
        console.log("submitted");
        this.editingForm = false;
      }

    },

    /*validInputCheck(inputValue){
      console.log(inputValue);
      console.log(this.v$.name.$dirty);
      console.log(this.v$.inputValue.$dirty);

      let loginInputClasses = 'input-form';

      if(!this.v$.inputValue.$dirty){
        return loginInputClasses;
      } 

      else if(this.v$.inputValue.$error){
        return loginInputClasses + ' invalid-input-class';
      }

      else if(!this.v$.inputValue.$invalid){
        return loginInputClasses + ' valid-input-class';
      }
    },*/


    // setLoginDivClasses(){this.loginDivClasses = this.loginDivClasses + "login-div";},
    // removeLoginDivClasses(){this.loginDivClasses = this.loginDivClasses.replace('login-div', '')},
    // setLoginButtonClasses(){this.loginButtonClasses = this.loginButtonClasses + "button-form";},
    // removeLoginButtonClasses(){this.loginButtonClasses = this.loginButtonClasses.replace('button-form', '')},
  },

  mounted() {
    // setTimeout(this.setLoginDivClasses, 100);
    // setTimeout(this.setLoginButtonClasses, 100);
  },

}

</script>


<template>
  <div :class="this.loginDivClasses">

      <form class="w-full max-w-xs bg-white flex flex-col py-5 px-8 rounded-lg shadow-lg" action="" @submit.prevent>

        <label class="label-form" for="">Nombre de usuario</label>
        <input :class="validNameInput" v-model="name" @blur="v$.name.$touch" type="text" placeholder="Nombre de usuario" :disabled="!this.editingForm">
        <p class="error-message" v-for= "error of v$.name.$errors" :key="error.$uid">{{error.$message}}</p>
 
        <label class="label-form" for="">Email</label>
        <input :class="validEmailInput" v-model="email" @blur="v$.email.$touch"  placeholder="Email" :disabled="!this.editingForm">
        <!-- <p v-for= "error of v$.$errors" :key="error.$uid">{{error.$message}}</p>  -->
        <p class="error-message" v-for= "error of v$.email.$errors" :key="error.$uid">{{error.$message}}</p>
    
        <label class="label-form" for="">Contraseña</label>
        <input :class="validPasswordInput" v-model="password" @blur="v$.password.$touch" type="password" placeholder="Contraseña" :disabled="!this.editingForm">
        <p class="error-message" v-for= "error of v$.password.$errors" :key="error.$uid">{{error.$message}}</p>

        <label class="label-form" for="">Confirmar password</label>
        <input :class="validSamePasswordInput" v-model="confirmPassword" @blur="v$.confirmPassword.$touch" type="password" placeholder="Confirmar contraseña" :disabled="this.v$.password.$error || !this.v$.password.$dirty || !this.editingForm">

        <p class="error-message" v-for= "error of v$.confirmPassword.$errors" :key="error.$uid">{{error.$message}}</p>  

        <div class="flex justify-between items-center my-4">

          <button :class="this.loginButtonClasses" @click="formMode">
            {{buttonFormText}}
          </button>

          <a class="forgot-password" href="#">
            Forgot Password?
          </a>
        </div>
      </form>

    </div>
</template>


<style>
  .login-div{
    @apply h-screen bg-slate-100 flex justify-center items-center
  }

  .label-form{
    @apply text-gray-700 font-bold py-2
  }

  .input-form{
    @apply text-gray-700 shadow border rounded 
    border-gray-300 focus:outline-none py-1 px-3 mb-3
  }
  
  .button-form{
    @apply bg-blue-500 hover:bg-blue-700 text-white font-bold rounded py-2 px-4
  }

  .forgot-password{
    @apply text-blue-600 hover:text-blue-800 font-bold
  }

  .invalid-input-class{
    @apply border-2 border-rose-500
  }

  .valid-input-class{
    @apply border-2 border-green-600
  }
  
  .error-message{
    @apply text-red-600 text-sm
  }

  .input-form-readonly{
    @apply bg-white
  }

</style>