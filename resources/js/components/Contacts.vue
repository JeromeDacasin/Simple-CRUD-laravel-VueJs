<template>
    <div>
      <h1>Add Contacts</h1>
      <form action="#" @submit.prevent="edit ? updateContact(contact.id) : createContact()">
          <div class="form-group">
              <label >Name</label>
              <input v-model="contact.name" type="text" name="name" class="form-control">
          </div>

          <div class="form-group">
              <label >Email</label>
              <input v-model="contact.email" type="text" name="email" class="form-control">
          </div>

          <div class="form-group">
              <label >Telephone/Cellphone Number</label>
              <input v-model="contact.phone" type="text" name="phone" class="form-control">
          </div>

          <div class="form-group">
              <button v-show="!edit" type="submit" class="btn btn-primary">New Contact</button>
              <button v-show="edit" type="submit" class="btn btn-primary">Update Contact</button>
          </div>
          
      </form>

      <h1>Contacts</h1>
      <table class="table table-striped">
          <thead class="thead-dark center">
              <th>Name</th>
              <th>Email</th>
              <th>Phone number</th>
              <th>Action</th>
          </thead>
          <tbody>
              <tr  v-for="contact in list" :key="contact.id" >
                  <td>{{contact.name}}</td>
                  <td>{{contact.email}}</td>
                  <td>{{contact.phone}}</td>
                  <td><button @click="showContact(contact.id)" class="btn btn-success btn-xs">Edit</button>
                      <button @click="deleteContact(contact.id)" class="btn btn-danger btn-xs">Delete</button></td>
                 
                  
              </tr>
          </tbody>

      </table>
    </div>
</template>


<script>
export default {
    data: function(){
        return {
            edit: false,
            list: [],
            contact: {
                id:'',
                email:'',
                phone:''
            }
        }
    },
    mounted: function(){
        console.log('Contacts Component loaded');
        this.fetchContactList();
    },
    methods: {
        fetchContactList: function(){
            console.log('fecthing contacts');
            axios.get('api/contacts')
            .then((response) => {
               
                this.list = response.data;
            })
            .catch((error) => {
                return error;
            });
        },
        createContact: function(){
            console.log('Creating Contact...');
            let self = this;
            Swal.fire({
                position: 'center',
                icon: 'success',
                title: 'Your work has been saved',
                showConfirmButton: false,
                timer: 1500
            })
            let params = Object.assign({}, self.contact)
             Swal.fire({
                position: 'center',
                icon: 'success',
                title: 'Successfully updated',
                showConfirmButton: false,
                timer: 1500
            })
            axios.post('api/contact/store', params)
            .then(function(){
                self.contact.name = '';
                self.contact.email = '';
                self.contact.phone = '';
                self.edit = false;
                self.fetchContactList();
            })
            .catch(function(error){
                console.log(error)
            });
           
        },
        showContact: function(id){
            let self = this;
           
            axios.get(`api/contact/${id}`)
            .then(function(response){
                console.log(response.data)
                self.contact.id = response.data.id;
                self.contact.name = response.data.name;
                self.contact.email = response.data.email;
                self.contact.phone = response.data.phone;
                
            })
            
            .catch(function(error){
                console.log(error)
            });
            
            self.edit = true;
        },
        updateContact: function(id){
            let self = this;
             Swal.fire({
                position: 'center',
                icon: 'success',
                title: 'Successfully updated',
                showConfirmButton: false,
                timer: 1500
            })
            let params = Object.assign({}, self.contact)
            axios.patch(`api/contact/${id}`, params)
            .then(function(){
                self.contact.name = '';
                self.contact.email = '';
                self.contact.phone = '';
                self.edit = false;
                self.fetchContactList();
            })
            .catch(function(error){
                console.log(error)
            });
        },
        deleteContact: function(id){
            let self = this;
            Swal.fire({
                position: 'center',
                icon: 'success',
                title: 'Data Deleted ',
                showConfirmButton: false,
                timer: 1500
            })
            axios.delete(`api/contact/${id}`)
            .then(function(response){
               self.fetchContactList();
            })
            .catch(function(error){
                console.log(error)
            })
        }

    }
}
</script>