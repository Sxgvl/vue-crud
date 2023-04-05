<script>
import axios from "axios";

export default {
  name: "student",
  data() {
    return{
      student: {
            name: "",
            course: "",
            email: "",
            phone: ""
        },
      studentList: [],
      users: [
      { id: 1, name: "peter", email: "peter@test.com" },
      { id: 2, name: "sam", email: "sam@test.com" },
      { id: 3, name: "alex", email: "alex@test.com" },
      { id: 4, name: "clover", email: "clover@test.com" },
      ]
    }

  },
  mounted() {
    console.log("Students are mounted.");
    this.getStudents();
  },
  methods: {
    //liste des etudiants
    getStudents() {
      axios.get('http://localhost:57672/students').then((response) => {
        this.studentList = response.data;
        console.log(response);
      });
    },
    //modifier un etudiant
    editStudent(id){
      // console.log(id, student);
        // return axios.put(`http://localhost:57672/students/${id}`, student).then(response => response.data);
        this.$router.push(`/students/${id}/edit`);
    },
    //supprimer un etudiant
    deleteStudent(id){
      if(window.confirm('Do you want to delete this student ?')){
        axios.delete(`http://localhost:57672/students/${id}`).then(() => {
          this.getStudents();
        }).catch(error => {
          console.log('An error occured while deleting this student')
        })
      }
    }
  },
};
</script>

<template>
  <div class="container">
    <div class="card">
      <div class="card-header">
        <h4>
          Students
          <RouterLink to="/students/create" class="btn btn-success float-end">
            Add student
          </RouterLink>
        </h4>
      </div>
      <div class="card-body">
        <table class="table table-bordered">
          <thead>
            <tr>
              <th>ID</th>
              <th>Name</th>
              <th>Course</th>
              <th>Email</th>
              <th>Phone</th>
              <th>Created at</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody v-if="studentList && studentList.length > 0">
            <tr v-for="studentItem in studentList" :key="studentItem.id">
              <td>{{ studentItem.id }}</td>
              <td>{{ studentItem.name }}</td>
              <td>{{ studentItem.course }}</td>
              <td>{{ studentItem.email }}</td>
              <td>{{ studentItem.phone }}</td>
              <td>{{ studentItem.created_at }}</td>
              <td>
                <button class="btn btn-warning" @click="editStudent(studentItem.id)">
                  Edit
                </button>
                <button class="btn btn-danger" @click="deleteStudent(studentItem.id)">
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
          <!-- <tbody v-else>
            <tr>
              <td colspan="7">Nothing to see here.</td>
            </tr>
          </tbody> -->
        </table>
      </div>
    </div>
  </div>
</template>
