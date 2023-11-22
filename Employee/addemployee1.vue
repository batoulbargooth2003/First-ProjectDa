<template>
  <div class="container">
    <div class="header d-flex justify-content-between align-items-center">
      <h1 class="logo fst-italic text-primary">Employees List</h1>
      <b-button variant="primary" @click="showModal = true">Add Employee</b-button>
    </div>

    <b-modal
      id="modal-prevent-closing"
      ref="modal"
      title="Add a new employee"
      @show="resetModal"
      @hidden="resetModal"
      v-model="showModal"
      @ok="handleSubmit"
    >
      <form ref="employeeForm">
        <b-form-group label="First Name" label-for="fname-input" :state="getInputState('fname')">
          <b-form-input id="fname-input" v-model="employee.firstName" required :state="getInputState('fname')"></b-form-input>
        </b-form-group>
        <b-form-group label="Last Name" label-for="lname-input" :state="getInputState('lname')">
          <b-form-input id="lname-input" v-model="employee.lastName" required :state="getInputState('lname')"></b-form-input>
        </b-form-group>
        <b-form-group label="Email Address" label-for="email-input" :state="getInputState('email')">
          <b-form-input id="email-input" v-model="employee.email" type="email" required :state="getInputState('email')"></b-form-input>
        </b-form-group>
        <b-form-group label="Password" label-for="pass-input" :state="getInputState('password')">
          <b-form-input type="password" id="pass-input" v-model="employee.password" required :state="getInputState('password')"></b-form-input>
          <b-form-invalid-feedback :state="getInputState('password')">
            Your password must be 8-20 characters long, contain letters and numbers, and must not contain spaces, special characters, or emoji.
          </b-form-invalid-feedback>
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
import employeelist1 from "@/components/employeelist1.vue";

export default {
  components: {
    BModal,
    BButton,
    BFormInvalidFeedback,
    BFormGroup,
    BFormInput,
    employeelist1,
  },
  data() {
    return {
      showModal: false,
      employees: [],
      employee: {
        id: 1,
        firstName: "",
        lastName: "",
        email: "",
        password: "",
      },
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
        id: 1,
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
        this.employees = JSON.parse(savedEmployees);
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