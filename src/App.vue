<template>
  <div>
    <h1 class="text-center mt-5">Employees</h1>
    <EmployeeForm @add:employee="addEmployee" />
    <Home :employees="employees" @delete:employee="deleteEmployee" @edit:employee="editEmployee" />
  </div>
</template>

<script>
import EmployeeForm from "./components/EmployeeForm.vue";
import Home from "./components/Home.vue";
export default {
  name: "App",
  components: {
    Home,
    EmployeeForm,
  },
  data() {
    return {
      employees: [],
    };
  },
  methods: {
    async addEmployee(employee) {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users',
          {
            method: 'POST',
            body: JSON.stringify(employee),
            headers: { 'Content-type': 'application/json; charset=UTF-8' }

          })
        const data = await response.json();

        this.employees = [...this.employees, data];
      }
      catch (error) {
        console.log(error);
      }
    },
    async deleteEmployee(id) {
      try {

        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`,
          {
            method: "DELETE"
          }
        )
        this.employees = this.employees.filter((employee) => {
          return employee.id !== id;
        })
      }
      catch (error) {
        console.log(error);
      }
    },
    async editEmployee(id, updatedEmployee) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`,
          {
            method: "PUT",
            body: JSON.stringify(updatedEmployee),
            headers: { 'Content-type': 'application/json; charset=UTF-8' }
          }
        )
        const data = await response.json();

        this.employees.map((employee) => {
          return employee.id == id ? data : employee;
        })
      }
      catch (error) {
        console.log(error);
      }
    },
    async getEmployees() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', { method: 'GET' });
        const data = await response.json();
        this.employees = data;
      } catch (error) {
        console.log("Error fetching employees", error);
      }
    }
  },
  mounted() {
    this.getEmployees();
  }
};
</script>



<style scoped></style>
