<template>
    <div class="div">
        <Header></Header>
        <div id="main">
            <div class="" id="cover-image">

                <div class="container">
                    <div class="row cover-text ">
                        <h3 class="text-light">Settings & Privacy</h3>
                    </div>

                </div>


            </div>
            <div class="container box ">
                <div class="row">
                    <div class="avater-image">
                        <img src="public/assets/images/faces/1.jpg" class="img-fluid " alt="">
                    </div>
                </div>
                <h5 class="text-danger text-capitalize" v-for="(error, index) in errors" :key="index">{{ error }}</h5>
                <h5 class="text-success text-capitalize" v-if="message != ''">{{ message }}</h5>
                <div class="row my-5">
                    <div class="col-12 col-lg-8">
                    
                        <div class="row">
                             <div class="section-header">
                                <h2>Settings</h2>
                            </div>
                             <div class="section-card my-3">
                                <div class="card  col-12" style=" padding:0px 20px;">
                                    <div class="row">
                                        <div class="col-lg-6 col-8">
                                            <div class="form-floating my-3">
                                                <input type="password" class="form-control" id="floatingPassword"  v-model="password" placeholder="Password">
                                                <label for="floatingPassword">Password</label>
                                            </div>
                                        </div>
                                       <div class="col-lg-6 col-4">
                                            <button  @click="updatePassword()" class="btn btn-primary btn-block btn-sm shadow-sm mt-4">Save</button>
                                        </div>
                                       <div class="col-lg-6 col-8">
                                             <div class="form-floating my-3">
                                                <input type="email" class="form-control" id="floatingInpuet"  v-model="email" placeholder="model@example.com">
                                                <label for="floatingInpuet">Email address</label>
                                            </div>
                                        </div>
                                        <div class="col-lg-6 col-4">                                   
                                                <button  @click="updateMail()" class="btn btn-primary btn-block btn-sm shadow-sm mt-4">Save</button>
                                        </div>
                                        <div class="col-lg-6 col-8">
                                            <div class="form-floating my-3">
                                                    <input type="text" class="form-control" id="floatingInputu" v-model="username" placeholder="dashh">
                                                    <label for="floatingInputu">User name</label>
                                                </div>
                                        </div>
                                         <div class="col-lg-6 col-4">                                   
                                                <button  @click="updateUsername()" class="btn btn-primary btn-block btn-sm shadow-sm mt-4">Save</button>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                   <div class="col-12 col-lg-4">
                    <div class="row my-5">

                            <div class="section-card my-3">
                                <div class="" style="width: 18rem;">
                                    <ul class="list-group list-group-flush logout" >
                                        <li class="list-group-item text-danger "><a href="/logout" class="text-danger" style="text-decoration: none;"><i class="bi bi-power  me-2"></i>Logout</a> </li>
                                    </ul>
                                </div>
                            </div>
                        </div>
                   </div>
                
               
                </div>
            

            </div>
        </div>
    </div>
  
</template>

<script>
import Header from '../components/Header.vue';
export default {
    name:'SettingsView',
    components:{
        Header
    },
    data (){
        return{
            email:'',
            password:'',
            username:'',
            count:0,
            errors:[],
            message:'',
            authtoken:''
        }
    },
    mounted(){
        this.getUserData();
    },
    methods:{
        getUserData() {
            // if (localStorage.getItem("authtoken") == null) { this.$router.push({ path: '/login' }); }
            this.authtoken = localStorage.getItem("authtoken");
            const headers = {
                'Authorization': `Bearer ${this.authtoken}`,

            };
            this.axios.get('/user/profile', { headers })
                .then((res) => {
                    // JSON responses are automatically parsed.
                    if (res.data.status == true) {
                        this.user = res.data.data
                        this.email = this.user.email
                        this.username = this.user.user_profile.username


                    } else {
                        this.errors.push(res.data.errors)
                    }
                })
                .catch((e) => {
                    this.errors.push(e);
                });
        },
        updatePassword(){
            const headers = {
                'Authorization': `Bearer ${this.authtoken}`,

            };
            let data = {
                user_id: this.user.id,
                password: this.password
            }
            this.axios.put('/user/update-password', data , { headers })
                .then((res) => {
                    // JSON responses are automatically parsed.
                    if (res.data.status == true) {
                        console.log(res.data)
                        this.message = res.data.data.message



                    } else {
                        this.errors.push(res.data.errors)
                    }
                })
                .catch((e) => {
                    this.errors.push(e);
                });
        },
        updateMail(){
            const headers = {
                'Authorization': `Bearer ${this.authtoken}`,

            };
            let data = {
                user_id: this.user.id,
                password: this.email
            }
            this.axios.put('/user/update-email', data, { headers })
                .then((res) => {
                    // JSON responses are automatically parsed.
                    if (res.data.status == true) {
                        console.log(res.data)
                        this.message = res.data.data.message



                    } else {
                        this.errors.push(res.data.errors)
                    }
                })
                .catch((e) => {
                    this.errors.push(e);
                });
        },
        updateUsername(){
           
            const headers = {
                'Authorization': `Bearer ${this.authtoken}`,

            };
            let data = {
                user_id: this.user.id,
                username: this.username
            }

            this.axios.put('/user/update-username', data, { headers })
                .then((res) => {
                    // JSON responses are automatically parsed.
                    if (res.data.status == true) {
                        this.message = res.data.message
                        


                    } else {
                        this.errors.push(res.data.message);
                        this.errors.push(res.data.errors);
                    }
                })
                .catch((e) => {
                    this.errors.push(e);
                });
        },
    }
}
</script>

<style>

</style>