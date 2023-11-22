<template>
    <div>
  <b-button v-b-modal.modal-1>ADD Employee</b-button>

  <b-modal id="modal-1" title="Add a new Employee">
   
    <!-- form -->
    <div>
  <b-form inline>
    <label class="sr-only" for="inline-form-input-name">FirstName</label>
    <b-form-input
      id="inline-form-input-name"
      class="mb-2 mr-sm-2 mb-sm-0"
      placeholder="Enter your FirstName"
      v-model="form.firstname"
    ></b-form-input>

    <label class="sr-only" for="inline-form-input-name">LastName</label>
    <b-form-input
      id="inline-form-input-name"
      class="mb-2 mr-sm-2 mb-sm-0"
      placeholder="Enter your LastName"
      v-model="form.lastname"
    ></b-form-input>

    <label class="sr-only" for="inline-form-input-name" typeof="email">Email</label>
    <b-form-input
      id="inline-form-input-name"
      class="mb-2 mr-sm-2 mb-sm-0"
      placeholder="Enter your email"
      v-model="form.email"
    ></b-form-input>

    <label class="label">Password</label>
      <div class="field has-addons">
         <div class="control is-expanded">
           <input v-if="showPassword" type="text" class="input" v-model="form.password" />
           <input v-else type="password" class="input" v-model="password">
          </div>
  </div>

    

    
  </b-form>
</div>
<!-- end form -->
  </b-modal>
</div>
    </template>

<script>
  
import {
  
  BFormInput,
  BModal,
  BButton,
  BInputGroup,


} from 'bootstrap-vue'

export default {
    data() {
    return {
      showPassword: false,
      password: null,
      emlist:[],
      form:{
          firstname:'',
          lastname:'',
          email:'',
          password:'',
          
        },
    };

  },
  computed: {
    buttonLabel() {
      return (this.showPassword) ? "Hide" : "Show";
    }
  },


    methods:{
    toggleShow() {
        this.showPassword = !this.showPassword;
    },
    saveemployee(){
    this.emlist.push({
        id:this.emlist.length+1,
        firstname:this.form.firstname,
        lastname:this.form.lastname,
        email:this.form.email,
        password:this.form.password,
        });
    

    window.localStorage.setItem('list',JSON.stringify(this.emlist)) ;
       // console.log(this.form)
       // this.form.name=''
       // this.form.description=''
       // this.form.startdate=''
       // this.form.status=''
     console.log(this.emlist)
      

     },
  }
};
</script>

