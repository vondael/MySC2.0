<script>
import axios from 'axios'
import Button from '../../Admin/AddUser/Button'
import Swal from 'sweetalert2'
import { OrbitSpinner } from 'epic-spinners'
// const swalWithBootstrap = Swal.mixin({
//     customClass: {
//         confirmButton: 'btn btn-success py-2 px-4',
//         cancelButton : 'btn btn-danger py-2 px-4 mx-3'
//     },
//     buttonsStyling: false
// })


export default {
    components: {
        OrbitSpinner,
        Button
    },
   data() {
       return {
            allTeams: '',
            teams: '',
            loading: false,
            showButton : false,
            team: '',
            mahasiswa: ''
       }
   },
   methods: {
       toggleAddTask() {
            this.showButton = !this.showButton
        },
        getMahasiswa() {
            axios.get('https://dev.alphabetincubator.id/mysc-backend/public/api/user/experience/user')
            .then(response => {
                console.log('mahasiswa', response)
                this.mahasiswa = response.data
            
            })
        },
        submit() {
            const data = {
                team: this.team,
            }
            // console.log(questData)
            Swal.fire({
                title: 'Are you sure ?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: 'teal',
                cancelButtonColor: 'red',
                confirmButtonText: 'Yes'
            })  .then(result => {
                    if(result.value) {
                        axios.post('https://dev.alphabetincubator.id/mysc-backend/public/api/create/team', data)
                            .then(response => {
                                console.log(response)
                                Swal.fire(
                                    'Success!',
                                    'Team has been Added',
                                )
                                    this.getTeams()
                                    this.team = ''
                            })
                            .catch(error => {
                                console.log(error)
                            })
                    }
            })
        },
        jointeam(id) {
            Swal.fire({
                title: 'Are you sure ?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: 'teal',
                cancelButtonColor: 'red',
                confirmButtonText: 'Yes'
            })  .then(result => {
                    if(result.value) {
                        axios.post('https://dev.alphabetincubator.id/mysc-backend/public/api/add/team/' + id)
                            .then(response => {
                                console.log(response)
                                Swal.fire(
                                    'Success!',
                                )
                                this.$router.push('/profile')
                            })
                            .catch(error => {
                                console.log(error)
                            })
                    }
            })
        },
        getTeams() {
        axios.get('https://dev.alphabetincubator.id/mysc-backend/public/api/teams')
            .then(response => {
                console.log(response)
                this.allTeams = response.data.dropdown_list
            })
        },
   },
   created() {
       this.getTeams();
       this.getMahasiswa();
   }
}

</script>

<template>
    <div class="content-wrapper">
        <div class="content-header">
            <div class="container-fluid">
                <div class="row mb-2">
                    <div class="col-sm-12" style="justify-content:space-between; display:flex;">
                        <h1 class="m-0 text-dark">All Teams</h1>
                        <Button 
                            @toggle-add-user="toggleAddTask" 
                            :text="showButton ? 'Close' : 'Add Team'" 
                            :color="showButton ? 'red' : 'green'" />
                    </div>
                    <div v-show="showButton" class="col-sm-12">
                        <div class="form-group">
                            <label>Name</label>
                            <input v-model="team" type="email" placeholder="Ex : Blueberry" class="form-control">
                        </div>
                        <div class="row">
                            <div class="col-12">
                                <button @click="submit()" class="btn btn-primary" type="submit">Submit</button>
                            </div>
                        </div>
                    </div>
                    
                </div>
            </div>
        </div>
        <div class="content">
            <div class="container-fluid">
                <div class="row">
                    <div class="col-md-12">
                        <div class="card">
                            <div class="overlay" v-if="loading">
                                <orbit-spinner style="display: block; margin: auto"
                                        :animation-duration="1200"
                                        :size="55"
                                        color="black"
                                        />
                            </div>
                            <div class="card-body table-responsive p-0">
                                <!-- <input type="text" v-model="search" style="margin-bottom:20px;" placeholder="search"> -->
                                <table class="table table-striped table-valign-middle">
                                    <thead>
                                        <tr>
                                            <th>No</th>
                                            <th>Name</th>
                                            <th>Total Members</th>
                                            <th></th>
                                            <th></th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="(value, length) in allTeams" :key="value.id">
                                            <td>{{ length + 1 }}.</td>
                                            <td>{{ value.team }}</td>
                                            <td>{{ value.total }}</td>
                                            <td>
                                                <button @click="jointeam(value.id)" class="btn btn-success">Join</button>
                                            </td>
                                            <td>
                                                <router-link :to="{name: 'show-members', params: {id: value.id}}">
                                                    Show Members
                                                </router-link>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                            <!-- Pagination -->
                            <!-- <div class="my-4"> 
                                <ul class="pagination pagination-md justify-content-center text-center">
                                    <li class="page-item"
                                        :class="dataArray.current_page === 1 ? 'disabled' : ''"
                                    >
                                        <a class="page-link" @click="prevPage">
                                            Previous
                                        </a>
                                    </li>
                                    <li class="page-link" style="background-color: inherit"> 
                                        {{ dataArray.current_page }} of {{ dataArray.last_page }}
                                    </li>
                                    <li  class="page-item" 
                                        :class="dataArray.current_page === dataArray.last_page ? 'disabled' : ''"
                                    >
                                        <a class="page-link" @click="nextPage">
                                            Next
                                        </a>
                                    </li>
                                </ul>
                            </div> -->
                        </div>
                    </div>
                </div>
            </div>
        </div>
         <a id="back-to-top" href="#" class="btn btn-primary back-to-top" role="button" aria-label="Scroll to top">
        <font-awesome-icon icon="chevron-up" />
    </a>
    </div>
</template>

<style>
a:hover {
    cursor: pointer;
}
@keyframes spinner {
    to { transform: rotate(360deg); }
}
.fa-spinner {
    animation: spinner 1s linear infinite;
}

.active {
    background-color: teal;
}
</style>