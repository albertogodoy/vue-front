<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>
                    Students
                    <RouterLink to="/students/create" class="btn btn-primary">Add Student</RouterLink>
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
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody v-if="this.students.length > 0">
                        <tr v-for="(student, index) in this.students" :key="index">
                            <td>{{ student.id }}</td>
                            <td>{{ student.name }}</td>
                            <td>{{ student.course }}</td>
                            <td>{{ student.email }}</td>
                            <td>{{ student.phone }}</td>
                            <td>
                                <RouterLink :to="{path:`/students/${student.id}/edit`}" class="btn btn-success">Edit</RouterLink>
                                <button type="button" @click="deleteStudent(student.id)" class="btn btn-danger float-end">Delete</button>
                            </td>
                        </tr>
                    </tbody>
                    <tbody v-else>
                        <tr>
                            <td colspan="6">Processing...</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>


<script>
import axios from 'axios'

export default {
    name: 'students',
    data() {
        return {
            id: '',
            students: []
        }
    },
    // Inicializador
    mounted() {
        this.getStudents();
    },
    methods: {
        // funcion para obtener registros en BD
        getStudents() {
            axios.get('http://127.0.0.1:8000/api/students').then(res => {
                this.students = res.data.students
                //console.log(this.students)
            });
        },
        // Funcion para eliminar registro
        deleteStudent(id){
            axios.delete(`http://127.0.0.1:8000/api/students/${id}/delete`).then(res => {
                alert(res.data.message)
                this.getStudents()
            }); 
        },
    },
}
</script>