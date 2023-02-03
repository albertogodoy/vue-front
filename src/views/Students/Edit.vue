<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Edit Students</h4>
            </div>
            <div class="card-body">
                <ul class="alert alert-danger" v-if="Object.keys(this.errorList).length > 0">
                    <li class="mb-0 ms-3" v-for="(error, index) in this.errorList" :key="index">
                        {{ error[0] }}
                    </li>
                </ul>
                <div class="mb-3">
                    <label for="name">Name:</label>
                    <input type="text" v-model="model.student.name" class="form-control" />
                </div>
                <div class="mb-3">
                    <label for="course">Course:</label>
                    <input type="text" v-model="model.student.course" class="form-control" />
                </div>
                <div class="mb-3">
                    <label for="email">Email:</label>
                    <input type="email" v-model="model.student.email" class="form-control" />
                </div>
                <div class="mb-3">
                    <label for="phone">Phone:</label>
                    <input type="text" v-model="model.student.phone" class="form-control" />
                </div>
                <div class="mb-3">
                    <button type="button" @click="updateStudent" class="btn btn-primary">Update</button>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
import axios from 'axios'
export default {
    name: 'studentEdit',
    data() {
        return {
            id: '',
            errorList: '',
            model: {
                student: {
                    name: '',
                    course: '',
                    email: '',
                    phone: '',
                }
            }
        }
    },
    mounted() {
        //console.log(this.$route.params.id)
        this.id = this.$route.params.id;
        this.getStudentData(this.$route.params.id);
    },
    methods: {
        getStudentData(id) {
            axios.get(`http://127.0.0.1:8000/api/students/${id}/edit`).then(res => {
                console.log(res.data)
                this.model.student = res.data
            }).catch(function (error) {
                if (error.response) {
                    if (error.response.status == 404) {
                        alert(error.response.data.message)
                    }
                }
            });
        },
        updateStudent() {
            var mythis = this;
            axios.put(`http://127.0.0.1:8000/api/students/${this.id}/edit`, this.model.student).then(res => {
                console.log(res.data)
                alert(res.data.message)
                this.errorList = ''
            }).catch(function (error) {
                if (error.response) {
                    if (error.response.status == 422) {
                        mythis.errorList = error.response.data.errors;
                    }

                    if (error.response.status == 404) {
                        alert(error.response.data.message)
                    }
                }
            });
        }
    },
}
</script>