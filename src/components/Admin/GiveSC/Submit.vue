<script>
import axios from 'axios'
import Swal from 'sweetalert2'
// import { FingerprintSpinner } from 'epic-spinners'

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
            name : this.$route.params.name,
            user_id : this.$route.params.id,
            sc: '',
            dosen_id: '',
            }
        },
    mounted() {
       
    },

    created() {
        axios.get('https://dev.alphabetincubator.id/mysc-backend/public/api/secretchamber/records/get/' + this.user_id)
        .then(response => {
            console.log(response)
        this.sc = parseInt(response.data.sc) + 1
      })
     
    },
    
    methods: {
        submit() {
            this.loading = true
            Swal.fire({
                text: "Are you sure ?",
                imageUrl: "https://lh3.googleusercontent.com/-_niVlPvfWVk/YHhNZZNpOMI/AAAAAAAABgE/sQDKxIcsyRIXwYmkMQTRHKu-smSQYUF-QCK8BGAsYHg/s0/2021-04-15.png?authuser=0",
                imageWidth: 150,
                imageHeight: 60,
                showCancelButton: true,
                confirmButtonColor: '#27a745',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Yes!',
                reverseButtons: true
            }).then(response => {
                if(response.value) {
                    let data = {
                        sc: this.sc,
                        user_id: this.user_id
                    }
                    console.log(data)
                    axios.post('https://dev.alphabetincubator.id/mysc-backend/public/api/secretchamber/givesc/records', data)
                        .then(response => {
                            console.log(response)
                            Swal.fire({
                                position: 'center',
                                // imageUrl: "https://lh3.googleusercontent.com/-L0L0yfE5VpA/XpfifMdyIXI/AAAAAAAABFU/ZrtQpPoKXHsAj0kgc70Gn8IwWsybi0nbACK8BGAsYHg/s0/2020-04-15.png",
                                // imageWidth: 150,
                                // imageHeight: 60,
                                icon: 'success',
                                title: 'Your result has been sended',
                                showConfirmButton: false,
                                timer: 1500
                            })
                            this.$router.push('/admin')
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
        }
    },
//     created () {
//         axios.get('https://sisplus.raharja.me/web/index.php?r=api/rep&key=$2y$10$6zeQKZ8dBvAOW1omT2ft6OJT7Iu34bHtgtqph.s5nwMDPv3IjxlRG&email=' + this.$store.state.user.email)
//         .then(response => {
//             // console.log(response)
//                 this.user = response.data.results
//                 console.log(this.user)
//             })
//     },
//     computed: {
//         name() {
//             return this.$store.state.user.name
//         },
//         email() {
//             return this.$store.state.user.email
//         }
//     }
}
</script>

<template>
        <div class="content">
            <div class="container-
            uid">
                <div class="row text-center">
                    <div class="col-md-6 mx-auto text-center" style="margin-top:90px;">
                        <div class="card card-olive">
                            <div class="card-header">
                                <h3 class="card-title">Submit SC</h3>
                            </div>
                            <div class="card-body">
                                <form role="form">
                                    <div class="row">
                                        <div class="col-sm-12">
                                            <div class="form-group">
                                                    <label>Nama</label>
                                                    <input disabled :placeholder="name" type="text" class="form-control">
                                            </div>
                                        </div>

                                        <div class="col-sm-12 mt-2">
                                            <div class="form-group">
                                                <label>SC</label>
                                                <input v-model="sc" disabled type="text" class="form-control">
                                                <p>*SC ke berapa *</p>
                                            </div>
                                        </div>
                                    </div>
                                </form>
                            </div>
                            <div class="wrapper" style="margin:auto;" v-if="loading">
                                <fingerprint-spinner
                                    :animation-duration="1500"
                                    :size="64"
                                    color="#ff1d5e"
                                    />
                            </div>
                            <div class="card-footer">
                                <button @click="submit" class="btn btn-primary" type="submit">Submit</button>
                            </div>
                        </div>

                    </div>
                </div>
            </div>
        </div>
</template>