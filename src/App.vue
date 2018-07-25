<template>
  <div id="app">
    <nav class="navbar navbar-dark bg-primary">
        <a href="/" class="navbar-brand">App de links</a>
    </nav>

    <div class="container">
      <div class="row mt-5">
        <div class="col-sm-4">
          <div class="card">
            <div class="card-header">
              <h2>Add new link</h2>
            </div>
            <div class="card-body">
              <form @submit.prevent="addWebsite" >
                <div class="form-group">
                  <input type="text" class="form-control" placeholder="Name" v-model="newWebsite.name">
                </div>
                <div class="form-group">
                  <input type="text" class="form-control" placeholder="Author" v-model="newWebsite.author">
                </div>
                <div class="form-group">
                  <input type="text" class="form-control" placeholder="URL" v-model="newWebsite.url">
                </div>
                <button type="submit" class="btn btn-primary">Save</button>
              </form>
            </div>
          </div>
        </div>
        <div class="col-sm-8 text-center">
            <div class="card">
              <div class="card-header">
               <h3>Links list</h3>       
              </div>
              <div class="card-body">
                <table class="table table-striped table-bordered">
                  <thead>
                    <tr>
                      <td>Name</td>
                      <td>Author</td>
                      <td>Options</td>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="w in websites">
                      <td><a :href="w.url" target="_blank">{{ w.name}}</a></td>
                      <td>{{ w.author }}</td>
                      <button class="btn btn-danger" @click="deleteWebsite(w)">Delete</button>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
        </div>
      </div>
    </div>
    
  </div>
</template>

<script>
import Firebase from 'firebase'
import config from './config'
const app = Firebase.initializeApp(config)
const dbApp = app.database()
let websitesRef = dbApp.ref('websites')

import toastr from 'toastr'

export default {
  name: 'app',
  firebase:{
    websites: websitesRef
  },
  data(){
    return{
      newWebsite:{
        name:'',
        author:'',
        url:''
      }
    }
  },
  methods:{
    addWebsite(){
      websitesRef.push(this.newWebsite)
      toastr.success('Sitio web Agregado')
      this.newWebsite.name = ''
      this.newWebsite.author = ''
      this.newWebsite.url = ''
    },
    deleteWebsite(website){
      if(confirm('Estas seguro de elinimar el sitio?')){
        websitesRef.child(website['.key']).remove()
        toastr.success('Sitio web eliminado')
      }
    }
  }
}
</script>

<style>
  #app{
    background: #8360c3;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #2ebf91, #8360c3);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #2ebf91, #8360c3); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  height: 100vh;
  }
</style>
