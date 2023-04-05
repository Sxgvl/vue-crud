<script>
import axios from "axios";

export default {
  name: "studentCreate",
  data() {
    return {
        errorList: '',
        student: {
            name: "",
            course: "",
            email: "",
            phone: ""
        }
    };
  },
  methods: {
    saveStudent() {
        try{
            axios.post('http://localhost:57672/students/create', this.student).then(response => {
                alert(`Student added : ${response.data.name}`);
                this.student.name = '';
                // alert('ok');
                this.student.email = '';
                //revenir sur la liste des etudiants
                this.$router.push('/students')
            })
        } catch(error) {
            console.error(error);
            alert('An error occured while register this student !')
        }
    },
  },
};
</script>

<template>
  <div class="container mt-5">
    <div class="card">
      <div class="card-header">
        <h4>Add students</h4>
      </div>
      <form class="card-body" @submit.prevent="saveStudent">
        <!-- ERREUR BANDEAU -->
        <ul class="alert alert-warning" v-if="Object.keys(errorList).length > 0 ">
            <li class="mb-0 ms-3" v-for="errorNew in errorList" :key="errorNew.id">
                {{ errorNew[0] }}
            </li>
        </ul>

        <div class="mb-3">
          <label for="">Name : </label>
          <input type="text" v-model="student.name" id="" class="form-control" />
        </div>
        <div class="mb-3">
          <label for="">Course : </label>
          <input type="text" v-model="student.course" id="" class="form-control" />
        </div>
        <div class="mb-3">
          <label for="">Email : </label>
          <input type="email" v-model="student.email" id="" class="form-control"/>
        </div>
        <div class="mb-3">
          <label for="">Phone : </label>
          <input type="tel" v-model="student.phone" id="" class="form-control"/>
        </div>
        <div class="mb-3">
          <button type="submit" class="btn btn-primary">
            Save
          </button>
        </div>
    </form>
    </div>
  </div>
</template>

<style></style>
