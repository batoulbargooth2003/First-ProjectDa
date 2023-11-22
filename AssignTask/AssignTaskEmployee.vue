<template>
    <div class="container">
      <div class="header d-flex justify-content-between align-items-center">
        <h1 class="logo fst-italic text-primary">Employees List</h1>
        <b-button variant="primary" @click="showModal = true">Assign Task To Employee</b-button>
      </div>
  
      <b-modal
        id="assign-task"
        ref="modal"
        title="Add a new task to employee"
        @show="resetModal"
        @hidden="resetModal"
        v-model="showModal"
        @ok="handleSubmit"
      >
        <form ref="employeeForm">
          <b-form-group label="employee Name" label-for="employee-input" :state="getInputState('fname')">
            <b-form-select v-model="form.employeeName" :options="EmloyeeList" class="form-control" 
          text-field="firstName" value-field="firstName"></b-form-select>
 </b-form-group>
 
          <b-form-group label="Task Name" label-for="task-input" >
            <b-form-select v-model="form.employeeTask" :options="TaskList" class="form-control" text-field="name" value-field="name" ></b-form-select>
 </b-form-group>
 <b-form-group
          label=" Start Date"
          label-for="date"
          invalid-feedback="date is required"
        >
        <b-form-input type='date' v-model="form.startdate" id="date"></b-form-input>
        </b-form-group>


        <b-form-group
          label="End Date"
          label-for="date"
          invalid-feedback="date is required"
        >
        <b-form-input type='date' v-model="form.startdate" id="date"></b-form-input>
        </b-form-group>


        </form>
      </b-modal>
  
      <div class="employee-cards">
        <employeelist1 :employee="employees" />
      </div>
    </div>
  </template>
  
  <script>
  import { BModal, BButton, BFormInvalidFeedback, BFormGroup, BFormInput } from "bootstrap-vue";
  
  
  export default {
    components: {
      BModal,
      BButton,
      BFormInvalidFeedback,
      BFormGroup,
      BFormInput,
      
    },
    data() {
      return {
        showModal: false,
        employees: [],
        employee: {
          firstName: "",
          lastName: "",
          email: "",
          password: "",
        },
        form:{
            employeeName:'',
            employeeTask:[],
            startDate:'',
            endDate:'',
         
        },
        EmloyeeTask:[],
        EmloyeeList:[],
        TaskList:[],
        formStates: {
          fname: null,
          lname: null,
          email: null,
          password: null,
        },
      };
    },
    watch: {
      employees: {
        handler() {
          this.saveEmployeesToLocalStorage();
        },
        deep: true,
      },
    },
    mounted() {
      this.retrieveEmployeesFromLocalStorage();
    },
    methods: {
      getInputState(fieldName) {
        return this.formStates[fieldName];
      },
      checkFormValidity() {
        // Form validation logic
        // Set the formStates according to the validation result
        let isValid = true;
        if (this.employee.firstName.trim() === "") {
          this.formStates.fname = false;
          isValid = false;
        } else {
          this.formStates.fname = true;
        }
        if (this.employee.lastName.trim() === "") {
          this.formStates.lname = false;
          isValid = false;
        } else {
          this.formStates.lname = true;
        }
        if (this.employee.email.trim() === "") {
          this.formStates.email = false;
          isValid = false;
        } else {
          this.formStates.email = true;
        }
        if (this.employee.password.trim() === "") {
          this.formStates.password = false;
          isValid = false;
        } else {
          this.formStates.password = true;
        }
        return isValid;
      },
      handleSubmit() {
        if (!this.checkFormValidity()) {
          return;
        }
  
        // Assign a new ID to the employee
        this.employee.id = this.employees.length + 1;
  
        // Push the new employee to the employees array
        this.employees.push({ ...this.employee });
        // Reset the form andclear the employee data
        this.resetModal();
      },
      resetModal() {
        this.showModal = false;
        this.employee = {
          firstName: "",
          lastName: "",
          email: "",
          password: "",
        };
        this.resetFormStates();
      },
      resetFormStates() {
        this.formStates = {
          fname: null,
          lname: null,
          email: null,
          password: null,
        };
      },
      saveEmployeesToLocalStorage() {
        localStorage.setItem("employees", JSON.stringify(this.employees));
      },
      retrieveEmployeesFromLocalStorage() {
        const savedEmployees = localStorage.getItem("employees");
        if (savedEmployees) {
          this.EmloyeeList = JSON.parse(savedEmployees);
        }
        const savedTask = localStorage.getItem("tasks");
        if (savedTask) {
          this.TaskList = JSON.parse(savedTask);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
  }
  
  .header {
    margin-bottom: 20px;
  }
  
  .logo {
    margin: 0;
  }
  
  .employee-cards {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    grid-gap: 20px;
    margin-top: 20px;
  }
  </style>