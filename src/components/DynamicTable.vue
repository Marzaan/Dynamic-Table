<template>
  <div v-show="displayy" class="container">
    <h2 v-if="!edit" class="mt-4 text-center">Add Data</h2>
    <h2 v-if="edit" class="mt-4 text-center">Update Data</h2>
      <form>
        <div class="form-group">
          <label for="name">Username</label>
          <input required
            type="text"
            placeholder="Karim Ahmed"
            v-model="name"
            class="form-control"
          />
          <div v-if="!hasName"><p class="text-danger">This field is required</p></div>
        </div>
        <div class="form-group">
          <label for="email">Email</label>
          <input required
            type="email"
            placeholder="example@gmail.com"
            v-model="email"
            class="form-control"
          />
          <div v-if="!hasEmail"><p class="text-danger">This field is required</p></div>
        </div>
        <div class="form-group">
          <label for="country">Country</label>
          <input required
            type="text"
            placeholder="Bangladesh"
            v-model="country"
            class="form-control"
          />
          <div v-if="!hasCountry"><p class="text-danger">This field is required</p></div>
        </div>
        <button type="submit" v-if="!edit" class="btn btn-dark" @click="onSubmit($event)">Submit</button>
        <button type="submit" v-if="edit" class="btn btn-success" @click="updateRow($event)">Update</button>
      </form>
    </div>
    <div class="container">
      <h1 class="mt-4 text-center">Dynamic Table</h1>
        <div text="center">
              <button type='button' class="btn btn-info" v-on:click="addNewData()">
                  <i class="fa fa-plus-circle"></i>Add Data
              </button>
        </div>
      <table class="table mt-2">
        <thead class="thead-dark"> 
          <tr>
            <th scope="col">#</th>
            <th scope="col">Username</th>
            <th scope="col">Email</th>
            <th scope="col">Country</th>
            <th scope="col">Action</th>
          </tr>
        </thead>
        <tbody> 
          <tr v-if="!allData.length">
            <th colspan="5" class="text-center"><h3>No Published Data</h3></th>
          </tr>
          <tr v-for="(entry, i) in allData" :key="i">
            <th scope="row">{{ ++i }}</th>
            <td>{{ entry.name }}</td>
            <td>{{ entry.email }}</td>
            <td>{{ entry.country }}</td>
            <td>
              <div class="btn-group">
                <button type="button" class="btn btn-info" @click="editRow(entry, i)">Edit</button>
                <button type="button" class="btn btn-danger" @click="deleteRow(entry, i)">Delete</button>              
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
</template>
<script>
import {required} from 'vuelidate/lib/validators'
export default {
  name: "DynamicTable",
    data: () => (
      { 
        ind: 0,
        name: "", 
        email: "", 
        country: "", 
        displayy: false,
        edit: false,
        hasName: true,
        hasEmail: true,
        hasCountry: true,
        allData: [] 
      }
    ),      
    validations: {
      name: {required},
      email: {required},
      country: {required}
    },
  methods: {
    onSubmit(e) {
      e.preventDefault();
      if(this.name && this.email && this.country){
        this.displayy = !this.displayy,
        this.allData.push({ 
          name: this.name, 
          email: this.email, 
          country: this.country });
          localStorage.setItem('allData', JSON.stringify(this.allData));
        this.clearForm();
      }
      if(!this.name){
        this.hasName = false;
      }
      if(!this.email){
        this.hasEmail = false;
      }
      if(!this.country){
        this.hasCountry = false;
      }
    },
    addNewData() {
      this.displayy = !this.displayy;
        this.hasName = true;
        this.hasEmail = true;
        this.hasCountry = true;
    },    
    editRow(entry, i){
      this.edit = !this.edit;
      this.displayy = !this.displayy;
      this.hasName = true;
      this.hasEmail = true;
      this.hasCountry = true;
      this.name = entry.name;
      this.email = entry.email;
      this.country = entry.country;
      this.ind = i-1;
    },
    updateRow(e){
      e.preventDefault();
      if(this.name && this.email && this.country){
        this.edit = !this.edit;
        this.displayy = !this.displayy;
        let tmp={
          name:this.name,
          email:this.email,
          country:this.country
        }
        this.allData[this.ind] = tmp;
        localStorage.setItem('allData', JSON.stringify(this.allData));
        let tmp2 = JSON.parse(localStorage.getItem('allData'));
        this.allData = tmp2;
        this.clearForm();
      }
      if(!this.name){
        this.hasName = false;
      }
      if(!this.email){
        this.hasEmail = false;
      }
      if(!this.country){
        this.hasCountry = false;
      }
    },
    deleteRow(entry, i) {
      var idx = this.allData.indexOf(entry);
      console.log(idx, i);
      if (idx > -1) {
        this.allData.splice(idx, 1);
      }
      localStorage.setItem('allData', JSON.stringify(this.allData));
    },
    clearForm() {
      this.name = "";
      this.email = "";
      this.country = "";
    },
    created(){
      let tmp = JSON.parse(localStorage.getItem('allData'));
      if(tmp == null){
        this.allData = [];
      }
      else{
        this.allData = tmp;
      }
    }
  },
};
</script>
