<script>
import axios from 'axios'
// import router from '../../../router'
import Swal from 'sweetalert2'
const swalWithBootstrap = Swal.mixin({
    customClass: {
        confirmButton: 'btn btn-success py-2 px-4',
        cancelButton : 'btn btn-danger py-2 px-4 mx-3'
    },
    buttonsStyling: false
})
export default {
    data() {
        return {
            all : '',
            pending: [],
            verified: [],
            rejected: '',
            search: '',
            list: '',
            redeemed: ''
        }
    },
    mounted() {
        this.allRecords();
        this.pendingRecords();
        this.verifiedRecords();
        this.rejectedRecords();
        this.listAIC();
        this.redeemedAIC();
    },
    methods: {
        allRecords() {
         //For showing all Records
            axios.get('https://dev.alphabetincubator.id/mysc-backend/public/api/secretchamber/records')
            .then(response => {
                console.log('all', response)
                this.all = response.data.Data
            })
        },
        pendingRecords() {
        //for showing all Pending Records
            axios.get('https://dev.alphabetincubator.id/mysc-backend/public/api/secretchamber/records/pending')
            .then(response => {
                console.log('pending', response)
                const dataRes = response.data
                this.pending = [].slice.call(dataRes).sort((a,b) => (a.created_at) > (b.created_at) ? 1 : -1)
                

            })
        },
        verifiedRecords() {
        //for showing all Verified Records
            axios.get('https://dev.alphabetincubator.id/mysc-backend/public/api/secretchamber/records/verified')
            .then(response => {
                console.log('verified', response)
                this.verified = response.data
            })
        },
        rejectedRecords() {
        //For showing all rejected Records
            axios.get('https://dev.alphabetincubator.id/mysc-backend/public/api/secretchamber/records/rejected')
            .then(response => {
                console.log('rejected', response)
                this.rejected = response.data
            })
        },
        listAIC() {
            axios.get('https://dev.alphabetincubator.id/mysc-backend/public/api/secretchamber/aic/show')
            .then(response => {
                console.log('aic', response)
                this.list = response.data.Data.Record
            })
        },
        redeemedAIC() {
            axios.get('https://dev.alphabetincubator.id/mysc-backend/public/api/secretchamber/redeemed/aic')
            .then(response => {
                console.log('redeemed aic',response)
                this.redeemed = response.data.Data.Record
            })
        },
        approveAll() {
            swalWithBootstrap.fire({
                title: 'Are you sure ?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonText: 'Yes',
                cancelButtonText: 'No',
                reverseButtons: true
            }).then(response => {
                if(response.value) {
                    let data = {
                        Approve: 1.0
                    }
                    console.log(data)
                    axios.post('https://dev.alphabetincubator.id/mysc-backend/public/api/secretchamber/records/all/feedbackApprove', data)
                        .then(response => {
                            console.log(response)
                            Swal.fire({
                                position: 'center',
                                imageUrl: "https://lh3.googleusercontent.com/-_niVlPvfWVk/YHhNZZNpOMI/AAAAAAAABgE/sQDKxIcsyRIXwYmkMQTRHKu-smSQYUF-QCK8BGAsYHg/s0/2021-04-15.png?authuser=0",
                                imageWidth: 150,
                                imageHeight: 60,
                                icon: 'success',
                                title: 'Your result has been sended',
                                showConfirmButton: false,
                                timer: 1500
                            })
                            this.allRecords();
                            this.pendingRecords();
                            this.verifiedRecords();
                            this.rejectedRecords();
                            this.listAIC();
                        })
                        .catch(error => console.log(error))
                }else if(response.dismiss === Swal.DismissReason.cancel) {
                    swalWithBootstrap.fire(
                        'Cancelled',
                        'Your result is still safe',
                        'error'
                    )
                }
            })
        //For Approved a Record
        },
        approve(id) {
            swalWithBootstrap.fire({
                title: 'Are you sure ?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonText: 'Yes',
                cancelButtonText: 'No',
                reverseButtons: true
            }).then(response => {
                if(response.value) {
                    let data = {
                        Approve: 1.0
                    }
                    console.log(data)
                    axios.post('https://dev.alphabetincubator.id/mysc-backend/public/api/secretchamber/records/' + id + '/feedbackApprove', data)
                        .then(response => {
                            console.log(response)
                            Swal.fire({
                                position: 'center',
                                imageUrl: "https://lh3.googleusercontent.com/-_niVlPvfWVk/YHhNZZNpOMI/AAAAAAAABgE/sQDKxIcsyRIXwYmkMQTRHKu-smSQYUF-QCK8BGAsYHg/s0/2021-04-15.png?authuser=0",
                                imageWidth: 150,
                                imageHeight: 60,
                                icon: 'success',
                                title: 'Your result has been sended',
                                showConfirmButton: false,
                                timer: 1500
                            })
                            this.allRecords();
                            this.pendingRecords();
                            this.verifiedRecords();
                            this.rejectedRecords();
                            this.listAIC();
                        })
                        .catch(error => console.log(error))
                }else if(response.dismiss === Swal.DismissReason.cancel) {
                    swalWithBootstrap.fire(
                        'Cancelled',
                        'Your result is still safe',
                        'error'
                    )
                }
            })
        //For Approved a Record
        },
        reject(id) {
        swalWithBootstrap.fire({
                title: 'Are you sure ?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonText: 'Yes',
                cancelButtonText: 'No',
                reverseButtons: true
            }).then(response => {
                if(response.value) {
                    let data = {
                        Reject: 1.0
                    }
                    console.log(data)
                    axios.post('https://dev.alphabetincubator.id/mysc-backend/public/api/secretchamber/records/' + id + '/feedbackReject', data)
                        .then(response => {
                            console.log(response)
                            Swal.fire({
                                position: 'center',
                                imageUrl: "https://lh3.googleusercontent.com/-_niVlPvfWVk/YHhNZZNpOMI/AAAAAAAABgE/sQDKxIcsyRIXwYmkMQTRHKu-smSQYUF-QCK8BGAsYHg/s0/2021-04-15.png?authuser=0",
                                imageWidth: 150,
                                imageHeight: 60,
                                icon: 'success',
                                title: 'Your result has been sended',
                                showConfirmButton: false,
                                timer: 1500
                            })
                            this.pendingRecords();
                            this.verifiedRecords();
                            this.rejectedRecords();
                        })
                        .catch(error => console.log(error))
                }else if(response.dismiss === Swal.DismissReason.cancel) {
                    swalWithBootstrap.fire(
                        'Cancelled',
                        'Your result is still safe',
                        'error'
                    )
                }
            })
        //For Approved a Record
        },
        approveaic(id) {
            swalWithBootstrap.fire({
                title: 'Are you sure ?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonText: 'Yes',
                cancelButtonText: 'No',
                reverseButtons: true
            }).then(response => {
                if(response.value) {
                    let data = {
                        Approve: 1.0
                    }
                    console.log(data)
                    axios.post('https://dev.alphabetincubator.id/mysc-backend/public/api/secretchamber/claimaic/' + id + '/aic', data)
                        .then(response => {
                            console.log(response)
                            Swal.fire({
                                position: 'center',
                                imageUrl: "https://lh3.googleusercontent.com/-_niVlPvfWVk/YHhNZZNpOMI/AAAAAAAABgE/sQDKxIcsyRIXwYmkMQTRHKu-smSQYUF-QCK8BGAsYHg/s0/2021-04-15.png?authuser=0",
                                imageWidth: 150,
                                imageHeight: 60,
                                icon: 'success',
                                title: 'Your result has been sended',
                                showConfirmButton: false,
                                timer: 1500
                            })
                            this.pendingRecords();
                            this.verifiedRecords();
                            this.rejectedRecords();
                            this.listAIC();
                        })
                        .catch(error => console.log(error))
                }else if(response.dismiss === Swal.DismissReason.cancel) {
                    swalWithBootstrap.fire(
                        'Cancelled',
                        'Your result is still safe',
                        'error'
                    )
                }
            })
        //For Approved a AIC
        },
        rejectaic(id) {
        swalWithBootstrap.fire({
                title: 'Are you sure ?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonText: 'Yes',
                cancelButtonText: 'No',
                reverseButtons: true
            }).then(response => {
                if(response.value) {
                    let data = {
                        Reject: 1.0
                    }
                    console.log(data)
                    axios.post('https://dev.alphabetincubator.id/mysc-backend/public/api/secretchamber/claimaic/' + id + '/aic', data)
                        .then(response => {
                            console.log(response)
                            Swal.fire({
                                position: 'center',
                                imageUrl: "https://lh3.googleusercontent.com/-_niVlPvfWVk/YHhNZZNpOMI/AAAAAAAABgE/sQDKxIcsyRIXwYmkMQTRHKu-smSQYUF-QCK8BGAsYHg/s0/2021-04-15.png?authuser=0",
                                imageWidth: 150,
                                imageHeight: 60,
                                icon: 'success',
                                title: 'Your result has been sended',
                                showConfirmButton: false,
                                timer: 1500
                            })
                            this.pendingRecords();
                            this.verifiedRecords();
                            this.rejectedRecords();
                            this.listAIC();
                        })
                        .catch(error => console.log(error))
                }else if(response.dismiss === Swal.DismissReason.cancel) {
                    swalWithBootstrap.fire(
                        'Cancelled',
                        'Your result is still safe',
                        'error'
                    )
                }
            })
        //For Approved a AIC
        },
        approveAicAll() {
            swalWithBootstrap.fire({
                title: 'Are you sure ?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonText: 'Yes',
                cancelButtonText: 'No',
                reverseButtons: true
            }).then(response => {
                if(response.value) {
                    let data = {
                        Approve: 1.0
                    }
                    console.log(data)
                    axios.post('https://dev.alphabetincubator.id/mysc-backend/public/api/secretchamber/claimaic/all', data)
                        .then(response => {
                            console.log(response)
                            Swal.fire({
                                position: 'center',
                                imageUrl: "https://lh3.googleusercontent.com/-_niVlPvfWVk/YHhNZZNpOMI/AAAAAAAABgE/sQDKxIcsyRIXwYmkMQTRHKu-smSQYUF-QCK8BGAsYHg/s0/2021-04-15.png?authuser=0",
                                imageWidth: 150,
                                imageHeight: 60,
                                icon: 'success',
                                title: 'Your result has been sended',
                                showConfirmButton: false,
                                timer: 1500
                            })
                            this.allRecords();
                            this.pendingRecords();
                            this.verifiedRecords();
                            this.rejectedRecords();
                            this.listAIC();
                        })
                        .catch(error => console.log(error))
                }else if(response.dismiss === Swal.DismissReason.cancel) {
                    swalWithBootstrap.fire(
                        'Cancelled',
                        'Your result is still safe',
                        'error'
                    )
                }
            })
        //For Approved a Record
        },
        
    },
    computed: {
            filteredVerified: function() {
                return this.verified.filter((value)=> {
                    return value.user.name.match(this.search)
                })
            },
            filteredPending: function() {
                return this.pending.filter((value)=> {
                    return value.user.name.match(this.search)
                })
            },
            filteredRejected: function() {
                return this.rejected.filter((value)=> {
                    return value.user.name.match(this.search)
                })
            },
        }
}
</script>

<template>
    <div class="wrapper">
        <div class="content-header">
            <div class="container-fluid">
                <div class="row mb-2">
                    <div class="col-sm-12"></div>
                </div>
                <div class="content">
                    <div class="container-fluid">
                        <div class="row">
                            <div class="col-lg-3 col-6">
                                <div class="small-box bg-info">
                                    <div class="inner">
                                        <h3>{{this.redeemed.length}}</h3>
                                        <p>Total Redeemed AIC</p>
                                    </div>
                                    <div class="icon">
                                        <i class="ion ion-bag"></i>
                                    </div>
                                    <!-- <a href="" class="small-box-footer">More Info
                                    <i class="fas fa-arrow-circle-right"></i>
                                    </a> -->
                                </div>
                            </div>
                            <div class="col-lg-3 col-6">
                                <div class="small-box bg-success">
                                    <div class="inner">
                                        <h3>{{this.all.Verified.length}}</h3>
                                        <p>Total Approved</p>
                                    </div>
                                    <div class="icon">
                                        <i class="ion ion-stats-bars"></i>
                                    </div>
                                    <!-- <a href="/admin/showecp" class="small-box-footer">More info 
                                    <i class="fas fa-arrow-circle-right">
                                        </i>
                                    </a> -->
                                </div>
                            </div>
                            <div class="col-lg-3 col-6">
                                <div class="small-box bg-warning">
                                    <div class="inner">
                                        <h3>{{ this.all.Pending.length }}</h3>
                                        <p>Total Pending</p>
                                    </div>
                                    <div class="icon">
                                        <i class="ion ion-person-add"></i>
                                    </div>
                                    <!-- <a href="/admin/showlevel" class="small-box-footer">More info <i class="fas fa-arrow-circle-right"></i>
                                    </a> -->
                                </div>
                            </div>
                            <div class="col-lg-3 col-6">
                                <div class="small-box bg-danger">
                                    <div class="inner">
                                        <h3>{{ this.rejected.length }}</h3>
                                        <p>Total Rejected</p>
                                    </div>
                                    <div class="icon">
                                        <i class="ion ion-person-add"></i>
                                    </div>
                                    <!-- <a href="/admin/showlevel" class="small-box-footer">More info <i class="fas fa-arrow-circle-right"></i>
                                    </a> -->
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="content" style="margin-top:5rem;">
                    <div class="container-fluid">
                        <div class="row">
                            <div class="col-12">
                                <div class="card">
                                    <div class="card-header p-2">
                                        <!-- <h3 class="card-title">Submitted SC</h3> -->
                                        <div class="card-tools">
                                            <ul class="nav nav-pills">
                                                <li class="nav-item">
                                                    <a class="nav-link active" href="#pending" data-toggle="tab">Pending</a>
                                                </li>
                                                <li class="nav-item">
                                                    <a class="nav-link" href="#approved" data-toggle="tab">Approved</a>
                                                </li>
                                                <li class="nav-item">
                                                    <a class="nav-link" href="#rejected" data-toggle="tab">Rejected</a>
                                                </li>
                                                 <li class="nav-item">
                                                    <a class="nav-link" href="#claimaic" data-toggle="tab">Redeem AIC</a>
                                                </li>
                                                <li class="nav-item">
                                                    <a class="nav-link" href="#redeemedaic" data-toggle="tab">Redeemed AIC</a>
                                                </li>
                                            </ul>
                                        </div>
                                    </div>
                                    <div class="card-body">
                                        <div class="tab-content">
                                            <div class="active tab-pane" id="pending">
                                                <div class="d-flex" style="justify-content:space-between;">
                                                    <input type="text" v-model="search" style="margin-bottom:20px;" placeholder="search">
                                                    <button type="button" @click="approveAll()" class="btn btn-success btn-sm mb-2">Approve All</button>
                                                </div>
                                                <table class="table">
                                                    <thead>
                                                        <tr>
                                                        <th>No</th>
                                                        <th>User</th>
                                                        <th>SC</th>
                                                        <th>Submitted</th>
                                                        <th>Status</th>
                                                        <th>Decision</th>
                                                        </tr>
                                                    </thead>
                                                    <tbody>
                                                        <tr v-for="(value, length) in filteredPending" :key="value.id">
                                                            <td>{{ filteredPending.length - length }}</td>
                                                            <td>{{ value.user.name }}</td>
                                                            <td>{{ value.sc }}</td>
                                                            <td>{{ value.created_at }}</td>
                                                            <td><span class="badge badge-warning">{{ value.status }}</span></td>
                                                            <td>
                                                                <div class="timeline-footer">
                                                                    <button type="button" @click="approve(value.id)" class="btn btn-success btn-sm mr-3">Approve</button>
                                                                    <button type="button" @click="reject(value.id)" class="btn btn-danger btn-sm">rejected</button>
                                                                </div>
                                                            </td>
                                                        </tr>
                                                    </tbody>
                                                </table>
                                            </div>
                                            <div class="tab-pane" id="approved">
                                                <input type="text" v-model="search" style="margin-bottom:20px;" placeholder="search">
                                                <table class="table">
                                                    <thead>
                                                        <tr>
                                                        <th>No</th>
                                                        <th>User</th>
                                                        <th>SC</th>
                                                        <th>Submitted</th>
                                                        <th>Status</th>
                                                        </tr>
                                                    </thead>
                                                    <tbody>
                                                        <tr v-for="(value, length) in filteredVerified" :key="value.id">
                                                            <td>{{ filteredVerified.length - length }}</td>
                                                            <td>{{ value.user.name }}</td>
                                                            <td>{{ value.sc }}</td>
                                                            <td>{{ value.created_at }}</td>
                                                            <td><span class="badge badge-success">{{value.status}}</span></td>
                                                        </tr>
                                                    </tbody>
                                                </table>
                                            </div>
                                             <div class="tab-pane" id="rejected">
                                                <input type="text" v-model="search" style="margin-bottom:20px;" placeholder="search">
                                                <table class="table">
                                                    <thead>
                                                        <tr>
                                                        <th>No</th>
                                                        <th>User</th>
                                                        <th>SC</th>
                                                        <th>Submitted</th>
                                                        <th>Status</th>
                                                        </tr>
                                                    </thead>
                                                    <tbody>
                                                        <tr v-for="(value, length) in filteredRejected" :key="value.id">
                                                            <td>{{ filteredRejected.length - length }}</td>
                                                            <td>{{ value.user.name }}</td>
                                                            <td>{{ value.sc }}</td>
                                                            <td>{{ value.created_at }}</td>
                                                            <td><span class="badge badge-danger">{{value.status}}</span></td>
                                                        </tr>
                                                    </tbody>
                                                </table>
                                            </div>
                                            <div class="tab-pane" id="claimaic">
                                                <button type="button" @click="approveAicAll()" class="btn btn-success btn-sm mb-2">Approve All</button>
                                                <table class="table">
                                                    <thead>
                                                        <tr>
                                                            <th>No</th>
                                                            <th>User</th>
                                                            <th>AIC</th>
                                                            <th>Status</th>
                                                            <th>No OVO</th>
                                                            <th>Submitted</th>
                                                            <th>Decision</th>
                                                        </tr>
                                                    </thead>
                                                    <tbody>
                                                        <tr v-for="(value, length) in list" :key="value.id">
                                                            <td>{{ list.length - length }}</td>
                                                            <td>{{ value.user.name }}</td>
                                                            <td>{{ value.value }}</td>
                                                            <td><span class="badge badge-warning">{{value.status}}</span></td>
                                                            <td>{{ value.rekening }}</td>
                                                            <td>{{ value.created_at }}</td>
                                                            <td>
                                                                <div v-show="value.status === 'pending' ">
                                                                    <button type="button" @click="approveaic(value.id)" class="btn btn-success btn-sm mr-3">Approve</button>
                                                                    <button type="button" @click="rejectaic(value.id)" class="btn btn-danger btn-sm">rejected</button>
                                                                </div>
                                                            </td>
                                                        </tr>
                                                    </tbody>
                                                </table>
                                            </div>
                                            <div class="tab-pane" id="redeemedaic">
                                                <button type="button" @click="approveAicAll()" class="btn btn-success btn-sm mb-2">Approve All</button>
                                                <table class="table">
                                                    <thead>
                                                        <tr>
                                                        <th>No</th>
                                                        <th>User</th>
                                                        <th>AIC</th>
                                                        <th>Status</th>
                                                        <th>No OVO</th>
                                                        <th>Submitted</th>
                                                        </tr>
                                                    </thead>
                                                    <tbody>
                                                        <tr v-for="(value, length) in redeemed" :key="value.id">
                                                            <td>{{ redeemed.length - length }}</td>
                                                            <td>{{ value.user.name }}</td>
                                                            <td>{{ value.value }}</td>
                                                            <td><span class="badge badge-success">{{value.status}}</span></td>
                                                            <td>{{ value.rekening }}</td>
                                                            <td>{{ value.created_at }}</td>
                                                        </tr>
                                                    </tbody>
                                                </table>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
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

<style scoped>
.small-box>.inner {
    padding: 10px;
}

.small-box h3 {
    font-size: 2.2rem;
    font-weight: 700;
    margin: 0 0 10px 0;
    padding: 0;
    white-space: nowrap;
}

.small-box {
    color: white;
}

@media only screen and (max-width: 600px) {
    p-button {
        margin-left: 10rem;
    }
}

@media only screen and (min-width: 600px) {
    p-button {
        margin-left: 10rem;
    }
}

.card-title {
    float: left;
    font-size: 1.1rem;
    font-weight: 400;
    margin: 0;
    padding-top: 10px;
}

.card-tools {
    float: right;
    margin-right: -.625rem;
}

.small-box>.small-box-footer {
    background: rgba(0,0,0,.1);
    color: rgba(255,255,255,.8);
    display: block;
    padding: 3px 0;
    position: relative;
    text-align: center;
    text-decoration: none;
    z-index: 10;
}
.reverseorder {
  display: flex;
  flex-direction: column-reverse;
}

.active {
    background-color: #ffff;
}
</style>


