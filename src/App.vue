<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>

    <employee-form @add:employee="addEmployee" /> <!-- beim event "add:employee" rufe die Methode "addEmployee" auf -->
    <employee-table v-bind:employees="employeeList" @delete:employee="deleteEmployee" @edit:employee="editEmployee"/>
  </div>
</template>

<script>
  import EmployeeTable from '@/components/EmployeeTable.vue'
  import EmployeeForm from '@/components/EmployeeForm.vue'

  export default {
    name: 'app',
    components: {
      EmployeeTable,
      EmployeeForm,
    },
    data() {
      return {
        /*employeeList: [
          {
            id: 1,
            name: 'Richard Hendricks',
            email: 'richard@piedpiper.com',
          },
          {
            id: 2,
            name: 'Bertram Gilfoyle',
            email: 'gilfoyle@piedpiper.com',
          },
          {
            id: 3,
            name: 'Dinesh Chugtai',
            email: 'dinesh@piedpiper.com',
          },
          {
            id: 4,
            name: 'klemensl',
            email: 'klemensl@piedpiper.com',
          },
        ],*/
        employeeList: [],
      }
    },
    mounted() {
      this.getEmployees() // wieso wirft das keinen Fehler im Linter, wenn die Methode nicht deklariert ist?
    },
    methods: {
      addEmployee(employee) {
        //this.employees = [...this.employees, employee] weil eine ID vergeben muss ist diese Zeile (leider) nicht ausreichend
        const lastId =
          this.employeeList.length > 0
            ? this.employeeList[this.employeeList.length - 1].id
            : 0;
        const id = lastId + 1;
        const newEmployee = { ...employee, id };

        this.employeeList = [...this.employeeList, newEmployee]; // kein weiteres "Update" oder dgl. notwendig, wird einfach i.d. Tabelle angezeigt
      },

      deleteEmployee(id) {
        this.employeeList = this.employeeList.filter(
          employee => employee.id !== id
        )
      },

      editEmployee(id, updatedEmployee) {
        this.employeeList = this.employeeList.map(employee =>
          employee.id === id ? updatedEmployee : employee
        )
      },
      async getEmployees() {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users')
          const data = await response.json()
          this.employeeList = data // und hier kein warning, wenn man nicht das in data deklarierte property verwendet
        } catch (error) {
          // eslint-disable-next-line no-console
          console.error(error)
        }
      }
    },
  }
</script>

<style>
  button {
    background: #009435;
    border: 1px solid #009435;
  }

  .small-container {
    max-width: 680px;
  }
</style>