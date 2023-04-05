<script>
import axios from "axios";

export default {
  name: "studentEdit",
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
  mounted(){
    this.getStudentData();
  },
  methods: {
    //recuperer les informations de l'etudiant selectionne
    getStudentData(){
        const id = this.$route.params.id;

        axios.get(`http://localhost:57672/students/${id}`).then(response => {
            //afficher dans la console les infos de l'etudiant a editer
            console.log(response.data);
            this.student = response.data
        }).catch(error => {
            console.log(error);
        });
    },
    editStudent() {
        try{
            const id = this.$route.params.id;

            axios.put(`http://localhost:57672/students/${id}`, this.student).then(response => {
                //grace a put on modifie les infos de l'etudiant et 
                //on revient sur la liste des etudiants
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
        <h4>Edit students</h4>
      </div>
      <form class="card-body" @submit.prevent="editStudent">
        <!-- ERREUR BANDEAU -->
        <ul class="alert alert-warning" v-if="Object.keys(errorList).length > 0 ">
            <li class="mb-0 ms-3" v-for="errorNew in errorList" :key="errorNew.id">
                {{ errorNew[0] }}
            </li>
        </ul>

        <div class="mb-3">
          <label for="">Name : </label>
          <input type="text" v-model="student.name" id="name" class="form-control" />
        </div>
        <div class="mb-3">
          <label for="">Course : </label>
          <input type="text" v-model="student.course" id="course" class="form-control" />
        </div>
        <div class="mb-3">
          <label for="">Email : </label>
          <input type="email" v-model="student.email" id="email" class="form-control"/>
        </div>
        <div class="mb-3">
          <label for="">Phone : </label>
          <input type="tel" v-model="student.phone" id="phone" class="form-control"/>
        </div>
        <div class="mb-3">
          <button type="submit" class="btn btn-warning">
            Edit
          </button>
        </div>
    </form>
    </div>
  </div>
</template>

<style></style>
