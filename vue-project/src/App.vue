<template>
  <div>
    <h1>Employee Data Grid</h1>
    <button @click="showModal()">Add Employee</button>
    <table>
      <thead>
        <tr>
          <th>Employee ID</th>
          <th>First Name</th>
          <th>Last Name</th>
          <th>DOB</th>
          <th>Salary</th>
          <th>Address</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tr v-for="(employee, index) in employees" :key="index">
        <td>{{ employee.employeeId }}</td>
        <td>{{ employee.firstName }}</td>
        <td>{{ employee.lastName }}</td>
        <td>{{ employee.dob }}</td>
        <td>{{ employee.salary }}</td>
        <td>{{ employee.address }}</td>
        <td>
          <button @click="editEmployee(index)">Edit</button>
          <button @click="deleteEmployee(index)">Delete</button>
        </td>
      </tr>
    </table>

    <div v-if="modalVisible" class="modal">
      <div class="modal-content">
        <span class="close" @click="closeModal">&times;</span>
        <h2>{{ modalTitle }}</h2>
        <form @submit.prevent="submitForm">
          <div>
            <label for="employeeId">Employee Id:</label>
            <input type="text" v-model="form.employeeId" required>
          </div>
          <div>
            <label for="firstName">First Name:</label>
            <input type="text" v-model="form.firstName" required>
          </div>
          <div>
            <label for="lastName">Last Name:</label>
            <input type="text" v-model="form.lastName" required>
          </div>
          <div>
            <label for="dob">DOB:</label>
            <input type="date" v-model="form.dob" required>
          </div>
          <div>
            <label for="salary">Salary:</label>
            <input type="number" v-model.number="form.salary" required>
          </div>
          <div>
            <label for="address">Address:</label>
            <input type="text" v-model="form.address" required>
          </div>
          <button type="submit">Submit</button>
          <button type="button" @click="hideModal">Cancel</button>
        </form>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      employees: [
        { employeeId: "1", firstName: "John", lastName: "Doe", dob: "1990-01-01", salary: 50000, address: "123 Main Street" },
        { employeeId: "2", firstName: "Jane", lastName: "Smith", dob: "1985-05-15", salary: 60000, address: "456 Elm Street" },
        { employeeId: "3", firstName: "Michael", lastName: "Thompson", dob: "1975-03-12", salary: 75000, address: "103 Maple Street" },
        { employeeId: "4", firstName: "Sarah", lastName: "Johnson", dob: "1992-07-24", salary: 68000, address: "406 Oak Avenue" },
        { employeeId: "5", firstName: "David", lastName: "Brown", dob: "1988-11-05", salary: 72000, address: "789 Pine Lane" },
        { employeeId: "6", firstName: "Emily", lastName: "Davis", dob: "1990-01-15", salary: 69000, address: "101 Elm Boulevard" },
        { employeeId: "7", firstName: "James", lastName: "Wilson", dob: "1983-04-22", salary: 76000, address: "202 Cedar Court" },
        { employeeId: "8", firstName: "Jessica", lastName: "Martinez", dob: "1987-08-18", salary: 71000, address: "330 Chestnut Boulevard" },
        { employeeId: "9", firstName: "Christopher", lastName: "Garcia", dob: "1991-02-27", salary: 68000, address: "303 Birch Drive" },
        { employeeId: "10", firstName: "Amanda", lastName: "Rodriguez", dob: "1993-05-10", salary: 67000, address: "550 Hickory Drive" },
        { employeeId: "11", firstName: "Joshua", lastName: "Lee", dob: "1984-09-30", salary: 74000, address: "404 Spruce Road" },
        { employeeId: "12", firstName: "Ashley", lastName: "Perez", dob: "1989-12-14", salary: 70000, address: "660 Juniper Road" },
        { employeeId: "13", firstName: "Matthew", lastName: "Thompson", dob: "1986-06-20", salary: 78000, address: "505 Willow Place" },
        { employeeId: "14", firstName: "Stephanie", lastName: "White", dob: "1994-10-07", salary: 66000, address: "220 Magnolia Lane" },
        { employeeId: "15", firstName: "Andrew", lastName: "Harris", dob: "1982-11-21", salary: 77000, address: "606 Cherry Circle" },
        { employeeId: "16", firstName: "Laura", lastName: "Clark", dob: "1995-03-29", salary: 68000, address: "909 Sycamore Street" },
        { employeeId: "17", firstName: "Daniel", lastName: "Lewis", dob: "1981-07-13", salary: 79000, address: "707 Walnut Way" },
        { employeeId: "18", firstName: "Megan", lastName: "Robinson", dob: "1986-10-25", salary: 71000, address: "440 Poplar Court" },
        { employeeId: "19", firstName: "Brandon", lastName: "Walker", dob: "1990-12-09", salary: 73000, address: "808 Aspen Terrace" },
        { employeeId: "20", firstName: "Melissa", lastName: "Young", dob: "1987-08-30", salary: 72000, address: "110 Redwood Avenue" },
      ],
      modalVisible: false,
      modalTitle: "Add Employee",
      editIndex: null,
      form: {
        employeeId: "",
        firstName: "",
        lastName: "",
        dob: "",
        salary: 0,
        address: ""
      }
    };
  },
  methods: {
    showModal(editIndex = null) {
      this.modalVisible = true;
      if (editIndex !== null) {
        this.modalTitle = "Edit Employee";
        this.editIndex = editIndex;
        this.form = { ...this.employees[editIndex] };
      } else {
        this.modalTitle = "Add Employee";
        this.editIndex = null;
        this.resetForm(); 
      }
    },
    closeModal() {
      this.modalVisible = false;
    },
    hideModal() {
      this.modalVisible = false;
    },
    resetForm() {
      this.form = {
        employeeId: "",
        firstName: "",
        lastName: "",
        dob: "",
        salary: 0, 
        address: ""
      };
    },
    submitForm() {
      this.form.salary = Number(this.form.salary);
      if (this.editIndex !== null) {
        this.employees[this.editIndex] = { ...this.form };
      } else {
        this.employees.push({ ...this.form });
      }
      this.closeModal();
      this.resetForm();
    },
    editEmployee(index) {
      this.showModal(index);
    },
    deleteEmployee(index) {
      this.employees.splice(index, 1);
    }
  }
};
</script>


<style scoped>
.modal {
  display: block;
  position: fixed; 
  z-index: 1; 
  left: 0;
  top: 0;
  width: 100%; 
  height: 100%; 
  overflow: auto; 
  background-color: rgb(0,0,0); 
  background-color: rgba(0,0,0,0.4); 
}

.modal-content {
  background-color: #fefefe;
  margin: 15% auto; 
  padding: 20px;
  border: 1px solid #888;
  width: 80%; 
}

.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
</style>