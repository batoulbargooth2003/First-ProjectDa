
<template>
    
    <div class="container">
        <employeelist2 :arry="employees"/>
        <div class="header d-flex justify-content-between align-items-center">
            <h1 class="logo fst-italic text-primary">Employees List</h1>
            <b-button v-b-modal.modal-prevent-closing variant="primary">Add Employee</b-button>
        </div>

        <b-modal id="modal-prevent-closing" ref="modal" title="Add a new employee" @show="resetModal" @hidden="resetModal"
            @ok="handleOk">
            <form ref="employeeForm" @submit.stop.prevent="handleSubmit">
                <b-form-group class="formGroup" label="First Name" label-for="fname-input"
                    invalid-feedback="First Name is required" :state="fnameState">
                    <b-form-input id="fname-input" v-model="fname" required :state="fnameState"></b-form-input>
                </b-form-group>
                <b-form-group class="formGroup" label="Last Name" label-for="lname-input"
                    invalid-feedback="Last Name is required" :state="lnameState">
                    <b-form-input id="lname-input" v-model="lname" required :state="lnameState"></b-form-input>
                </b-form-group>
                <b-form-group class="formGroup" label="Email Address" label-for="email-input"
                    invalid-feedback="Email Address is required" :state="emailState">
                    <b-form-input id="email-input" v-model="email" type="email" required></b-form-input>
                </b-form-group>
                <b-form-group class="formGroup" label="Password" label-for="pass-input"
                    invalid-feedback="Password is required" :state="emailState">
                        <b-form-input type="password" id="pass-input"
                            aria-describedby="password-help-block"></b-form-input>
                        <b-form-text id="password-help-block">
                            Your password must be 8-20 characters long, contain letters and numbers, and must not
                            contain spaces, special characters, or emoji.
                        </b-form-text>
                </b-form-group>
            </form>
            {{ fullname }}
        </b-modal>
        {{ arr }}
    </div>
</template>

<style>
.formGroup {
    margin-bottom: 40px;
}
</style>
  
<script>
import {
    BModal,
    BForm,
    BFormGroup,
    BFormCheckbox,
    BFormInput,
    BButton,
    BFormTextarea,
    BFormDatepicker,
    BFormSelect,
    BFormSelectOption,
    BTable
} from "bootstrap-vue"
import HelloWorld from "@/components/HelloWorld.vue"



export default {
    components: {
    BModal,
    BForm,
    BFormGroup,
    BFormCheckbox,
    BFormInput,
    BButton,
    BFormTextarea,
    BFormDatepicker,
    BFormSelect,
    BFormSelectOption,
    BTable,
    HelloWorld,
   
   
},
props:{
    arry:Object,
},
    data() {
        return {
            fname: '',
            fnameState: null,
            lname: '',
            lnameState: null,
            email: '',
            emailState: null,
            pass: '',
            passState: null,
            employees: [],
           
           
        }
    },
    mounted() {
        this.employees = JSON.parse(window.localStorage.getItem("Employees")) || [];
    },
    methods: {
        checkFormValidity() {
            const valid = this.$refs.employeeForm.checkValidity()
            this.fnameState = valid;
            this.lnameState = valid;
            this.emailState = valid;
            this.passState = valid;
            return valid;
        },
        resetModal() {
            this.fname = '';
            this.lname = '';
            this.email = '';
            this.pass = '';
            this.fnameState= null;
            this.lnameState= null;
            this.emailState= null;
            this.passState= null;
        },
        handleOk(bvModalEvent) {
            // Prevent modal from closing
            bvModalEvent.preventDefault()
            // Trigger submit handler
            this.handleSubmit()
        },
        handleSubmit() {
            // Exit when the form isn't valid
            if (!this.checkFormValidity()) {
                return
            }
            // Push the name to submitted names
            this.employees.push({
                id: this.employees.length + 1,
                firstName: this.fname,
                lastName: this.lname,
                email: this.email,
                password: this.pass,
                fullname:this.fullname
            });

            // add the array of tasks to local storage
            window.localStorage.setItem("Employees", JSON.stringify(this.employees));


            // Hide the modal manually
            this.$nextTick(() => {
                this.$bvModal.hide('modal-prevent-closing')
            })
        },
        editTask(task) {
            // this.$router.push(/edit/${task.id});
        }
    },
    computed:{
        fullname :function(){
            return `fullname : ${this.fname} ${this.lname}` 
        }
    }
}
</script>