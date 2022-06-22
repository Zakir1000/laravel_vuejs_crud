<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12">
                <div class="card">
                    <div class="card-header">
                        <div class="row">
                            <div class="col">
                                <h3>Customer Component</h3>

                            </div>
                            <div class="col">
                                <div class="float-right" style="margin-left:370px;">
                                    <button  type="button" class="btn btn-primary" @click="create">Add New Customer
                                        
                                    </button>
                                   <button @click="reload" type="submit" class="btn btn-success">Reload</button>
                                </div>
                                   
                            </div>
                        </div>
                        
                        
                    </div>

                    <div class="m-3">
                        <div class="row">
                            <div class="col-md-2">
                                <strong>Search By: </strong>

                            </div>
                            <div class="col-md-3">
                                <div class="form-group">
                                    <select v-model="queryField" class="form-control" name="" id="">
                                        <option value="name">NAME</option>
                                        <option value="email">EMAIL</option>
                                        <option value="phone">PHONE</option>
                                        <option value="address">ADDRESS</option>
                                        <option value="total">TOTAL</option>
                                    </select>
                                </div>
                            </div>
                            <div class="col-md-7">
                                <input type="text" v-model="query" class="form-control" placeholder="Search">
                            </div>
                        </div>
                    </div>

                    <div class="card-body">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th>ID</th>
                                    <th>IMAGE</th>
                                    <th>NAME</th>
                                    <th>EMAIL</th>
                                    <th>PHONE</th>
                                    <th>ADDRESS</th>
                                    <th>TOTAL</th>
                                    <th>ACTION</th>
                                </tr>

                            </thead>

                            <tbody>
                                <tr v-for="(customer, index) in customers" :key="customer.id">
                                    <td>{{index+1}}</td>
                                     <td v-if="customer.image !=null">
                                       <img style="width:120px;" :src="'../storage/customer/'+customer.image" :alt="customer.image">
                                   </td>
                                   <td v-else>
                                       <img style="width:120px;" :src="'../storage/customer/img1.jpg'" :alt="customer.image">
                                   </td>
                                    <td>{{customer.name}}</td>
                                    <td>{{customer.email}}</td>
                                    <td>{{customer.phone}}</td>
                                    <td>{{customer.address}}</td>
                                    <td>{{customer.total}}</td>
                                    <td>
                                        <button @click.prevent="edit(customer)" type="submit" class="btn btn-primary">Edit</button>
                                        <button @click.prevent="deletedata(customer)" type="submit" class="btn btn-danger">Delete</button>
                                    </td>
                                </tr>

                            </tbody>

                        </table>

                          <pagination v-if="pagination.last_page > 1"
                            :pagination="pagination"
                            :offset="5"
                            @paginate= "query === '' ? getData() : searchData()"
                            ></pagination>
                    </div>
                </div>
            </div>
        </div>
        <vue-progress-bar></vue-progress-bar>
        <vue-snotify></vue-snotify>
 

            <!-- Modal -->
            <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">

                    <form @submit.prevent="store()" method="post">
                        <div class="form-group">
                            <label for="name">Name</label>
                            <input type="text" class="form-control" v-model="form.name" name="name" id="name" placeholder="Enter name">
                        </div>

                        <div class="form-group">
                            <label for="email">Email</label>
                            <input type="text" class="form-control" v-model="form.email" name="email" id="email" placeholder="Enter email">
                        </div>

                       

                         <div class="form-group">
                            <label for="phone">Phone</label>
                            <input type="number" class="form-control" v-model="form.phone" name="phone" id="phone" placeholder="Enter phone">
                        </div>

                         <div class="form-group">
                            <label for="address">Address</label>
                            <textarea class="form-control" name="address" v-model="form.address" id="address"  rows="3"></textarea>
                        </div>

                        <div class="form-group">
                            <label for="total">Total</label>
                            <input type="number" class="form-control" v-model="form.total" name="total" id="total" placeholder="Enter total amount">
                        </div>

                        <div class="form-group">
                            <label for="image">Image</label><br>
                            <input type="file" @change="onFileChange" class="form-control-file" name="file" id="file" placeholder="image">
                            <img style="width:150px;" :src="imagePreview" alt="">
                        </div>

                        <div class="modal-footer">
                            <button type="reset" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                            <button type="submit" class="btn btn-primary">Save changes</button>
                        </div>

                    </form>
                </div>
                
                </div>
            </div>
            </div>


            <!-- Edit Modal -->
            <div class="modal fade" id="EditModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">

                    <form @submit.prevent="update()" method="post">
                        <div class="form-group">
                            <label for="name">Name</label>
                            <input type="text" class="form-control" v-model="form.name" name="name" id="name" placeholder="Enter name">
                        </div>

                        <div class="form-group">
                            <label for="email">Email</label>
                            <input type="text" class="form-control" v-model="form.email" name="email" id="email" placeholder="Enter email">
                        </div>

                       

                         <div class="form-group">
                            <label for="phone">Phone</label>
                            <input type="number" class="form-control" v-model="form.phone" name="phone" id="phone" placeholder="Enter phone">
                        </div>

                         <div class="form-group">
                            <label for="address">Address</label>
                            <textarea class="form-control" name="address" v-model="form.address" id="address"  rows="3"></textarea>
                        </div>

                        <div class="form-group">
                            <label for="total">Total</label>
                            <input type="number" class="form-control" v-model="form.total" name="total" id="total" placeholder="Enter total ammount">
                        </div>

                         <div class="form-group">
                            <label for="image">Image</label><br>
                            <input type="file" @change="onFileChange" class="form-control-file" name="file" id="file" placeholder="image">
                            <img v-bind:src="imagePreview == null ? '../storage/customer/'+form.image : imagePreview" width="100" height="100"/>
                        </div>

                        

                        <div class="modal-footer">
                            <button type="reset" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                            <button type="submit" class="btn btn-primary">Save changes</button>
                        </div>

                    </form>
                </div>
                
                </div>
            </div>
            </div>
    </div>
</template>

<script>

// import Vue from 'vue'
import Form from 'vform'
    export default {

        data(){
            return{
                imagePreview: null,
                query: "",
                queryField: "name",
                customers: [],
                pagination: {
                        current_page:1,
                },

                form: new Form({
                    id: "",
                    name: "",
                    email: "",
                    phone: "",
                    address: "",
                    total: "",
                    image: "",
                    

                }),
            }

        },
        watch: {
            query: function(newQ, old)
            {
                if (newQ === "") {
                    this.getData();
                    
                } else {
                    this.searchData();
                    
                }

            }

        },
        mounted() {
            console.log('Component mounted.');
            this.getData();
        },

        methods:{
            getData(){
                this.$Progress.start()
                 axios.get('/api/customer?page='+this.pagination.current_page)
                .then(response =>{
                    this.customers = response.data.data;
                    this.pagination = response.data.meta
                    console.log(response)
                    this.$Progress.finish()

                })
                .catch(e=>{
                    console.log(e)
                    this.$Progress.fail()

                })
            },

             searchData(){
                this.$Progress.start()
                 axios.get('/api/search/customer'+ '/' + this.queryField + '/' + this.query+ '?page=' + this.pagination.current_page)
                .then(response =>{
                    this.customers = response.data.data;
                    this.pagination = response.data.meta
                    console.log(response)
                    this.$Progress.finish()

                })
                .catch(e=>{
                    console.log(e)
                    this.$Progress.fail()

                })
            },

            reload()
            {
                this.$Progress.start()
                this.getData();
                this.query = "";
                this.queryField = "name";
                this.$snotify.success("Data successfully Refresh");
            },

            create()
            {
                this.form.reset();
                this.form.clear();
                $('#exampleModal').modal("show");
               

            },

            onFileChange(event){
                this.form.image = event.target.files[0];

                let reader  = new FileReader();
                reader.addEventListener("load", function () {
                    this.imagePreview = reader.result;
                }.bind(this), false);
                if( this.form.image ){
                    if ( /\.(jpe?g|png|gif)$/i.test( this.form.image.name ) ) {
                    reader.readAsDataURL( this.form.image );
                    }
                }

            },
            store(){
                this.$Progress.start();
                this.form.busy = true;
                this.form.post('/api/customer')
                .then(response=>{
                    this.getData();
                    $('#exampleModal').modal("hide");
                    if(this.form.successful){
                        this.$Progress.finish();
                        this.$snotify.success("Customer successfully saved");
                    }else{
                        this.$Progress.fail();
                        this.$snotify.error("something went wrong try again", "Error");


                    }

                    console.log(response)

                }).catch(error=>{

                })

            },

            edit(customer){
                 this.form.reset();
                this.form.clear();
                this.form.fill(customer);
                $('#EditModal').modal("show");
            },

            update(){
                this.$Progress.start();
                this.form.busy = true;
                this.form.post('/api/customer/'+this.form.id)
                .then(response=>{
                    this.getData();
                    $('#EditModal').modal("hide");
                    if(this.form.successful){
                        this.$Progress.finish();
                        this.$snotify.success("Customer successfully updated");
                    }else{
                        this.$Progress.fail();
                        this.$snotify.error("something went wrong try again", "Error");
                    }

                    console.log(response)

                }).catch(error=>{

                })

            },

            deletedata(customer){
                this.$snotify.clear();
                this.$snotify.confirm(
                    'you can not delete this data',
                    'are you sure?', {
                timeout: 5000,
                showProgressBar: true,
                closeOnClick: false,
                pauseOnHover: true,
                buttons: [
                    {text: 'Yes', 
                    action: (toast) =>  {
                            this.$snotify.remove(toast.id);
                            this.$Progress.start();
                            this.form.delete('/api/customer/'+customer.id)
                            .then(response=>{
                                this.getData();
                               
                                this.$Progress.finish();
                                this.$snotify.success("Customer deleted successfully");
                                console.log(response)
                              
                            }).catch(error=>{
                                console.log(error)

                            });


                       },
                      bold: true
                    },

                    {text: 'No', action: () => console.log('Clicked: No')},
                    {text: 'Later', action: (toast) => {console.log('Clicked: Later'); this.$snotify.remove(toast.id); } },
                    {text: 'Close', action: (toast) => {console.log('Clicked: No'); this.$snotify.remove(toast.id); }, bold: true},
                ]
                });

            },



        },


    }
</script>
