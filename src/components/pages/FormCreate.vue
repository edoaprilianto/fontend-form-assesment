<template>
    <layout-div>
      <h2 class="text-center mt-5 mb-3">Assesment Form Input</h2>
      <div class="card">

          
          <div class="card-body">
            <ul id="error" v-if="showErr">
                <li v-for="row in msgErr" :value="row.msg" :key="row">
                 <span class="text-danger">{{row.msg}}</span>
                </li>
            </ul>
              <form v-if="showForm">
                  <div class="form-group">
                      <label htmlFor="name">Name</label>
                      <input 
                          v-model="project.name"
                          type="text"
                          class="form-control"
                          id="name"
                          :require=true
                          name="name"/>
                  </div>
                  <div class="form-group">
                      <label htmlFor="name">Email</label>
                      <input 
                          v-model="project.email"
                          type="text"
                          class="form-control"
                          id="email"
                          :require=true
                          name="email"/>
                  </div>
                  <div class="form-group">
                      <label htmlFor="name">Phone Number</label>
                      <input 
                          v-model="project.phone_number"
                          type="text"
                          class="form-control"
                          id="number"
                          :require=true
                          name="number"/>
                  </div>
                  <div class="form-group">
                      <label htmlFor="bod">Born of Date</label>
                      <input 
                          v-model="project.date_of_born"
                          type="date"
                          class="form-control"
                          id="bod"
                          :require=true
                          name="bod"/>
                  </div>
                  <div class="form-group">
                      <label htmlFor="pod">Place of Date</label>
                      <input 
                          v-model="project.place_of_born"
                          type="text"
                          class="form-control"
                          id="pod"
                          :require=true
                          name="pod"/>
                  </div>
                  <div class="form-group">
                      <label htmlFor="pod">Sex</label>
                      <select class="form-control" v-model="project.sex" :require="true">
                        <option  v-for="item in sexs" :value="item" :key="item">{{ item }}</option>
                    </select>
                  </div>
                  <button 
                      @click="save()"
                      :disabled="isSaving"
                      type="button"
                      class="btn btn-outline-primary mt-3">
                      Save
                  </button>
              </form>
              <div v-if="showTable">
                <table class="table table-bordered">
                    <thead>
                        <th> Name </th>
                        <th> Email </th>
                        <th> Phone Number </th>
                        <th> Date of Born </th>
                        <th> Place of Born </th>
                        <th> Sex </th>
                    </thead>
                    <tbody>
                        <tr v-for="item in data" :value="item.name" :key="item">
                            <td> {{item.name}} </td>
                            <td> {{item.email}} </td>
                            <td> {{item.phone_number}} </td>
                            <td> {{item.date_of_born}} </td>
                            <td> {{item.place_of_born}} </td>
                            <td> {{item.sex}} </td>
                        </tr>
                    </tbody>
                </table>
                <button 
                      @click="back()"
                      type="button"
                      class="btn btn-outline-primary mt-3">
                      Back
                  </button>
             </div>
          </div>
      </div>
    </layout-div>
  </template>
   
  <script>
  import axios from 'axios';
  import LayoutDiv from '../LayoutDiv.vue';
  import Swal from 'sweetalert2'
   
  export default {
    name: 'ProjectCreate',
    components: {
      LayoutDiv,
    },
    data() {
      return {
        showErr: false,
        showTable: false,
        showForm: true,
        msgErr:[],
        data: [],
        project: {
        //   name: null,
        //   email: null,
        //   phone_number: null,
        //   date_of_born: null,
        //   place_of_born: null,
        //   sex: null
          name: 'Edo Aprilianto',
          email: 'edoapriliano70@gmail.com',
          phone_number: '081908802234',
          date_of_born: '05/04/1998',
          place_of_born: 'jakarta',
          sex: 'Male'
        },
        sexs : ['Male' ,'Female'],
        isSaving:false,
      };
    },
    methods: {
        save() {

          if(this.validation()){
            this.isSaving = true
            axios.post('/form', this.project)
                .then(response => {
                Swal.fire({
                    icon: 'success',
                    title: 'saved successfully!',
                    showConfirmButton: false,
                    timer: 1500
                })
                this.isSaving = false
                this.showTable = true;
                this.showForm = false;
                this.data.push(this.project)
                return response
                })
                .catch(error => {
                console.log(error.response.data)
                this.msgErr = error.response.data.message
                this.isSaving = false
                this.showErr = true
                this.showForm = true
                this.showTable = false;
                Swal.fire({
                    icon: 'error',
                    title: 'An Error Occured!',
                    showConfirmButton: false,
                    timer: 1500
                })
                return error
                });
          }
      },
      back(){
        this.showTable = false
        this.showForm = true
      },
      validation(){
        console.log(this.project.phone_number)
        if(this.project.name === null){
            Swal.fire({
                  icon: 'error',
                  title: 'Name is Required!',
                  showConfirmButton: false,
                  timer: 1500
              })
              return false;
        }
        if(this.project.email === null){
            Swal.fire({
                  icon: 'error',
                  title: 'Email is Required!',
                  showConfirmButton: false,
                  timer: 1500
              })
              return false;

        }
        if(this.project.phone_number === null){
            Swal.fire({
                  icon: 'error',
                  title: 'Phone Number is Required!',
                  showConfirmButton: false,
                  timer: 1500
              })
              return false;

        }
        if(this.project.date_of_born === null){
            Swal.fire({
                  icon: 'error',
                  title: 'Born of Date is Required!',
                  showConfirmButton: false,
                  timer: 1500
              })
              return false;

        }
        if(this.project.place_of_born === null){
            Swal.fire({
                  icon: 'error',
                  title: 'Place of Date is Required!',
                  showConfirmButton: false,
                  timer: 1500
              })
              return false;

        }
        if(this.project.sex === null){
            Swal.fire({
                  icon: 'error',
                  title: 'Sex is Required!',
                  showConfirmButton: false,
                  timer: 1500
              })
              return false;

        }
        return true
      }
    },
  };
  </script>