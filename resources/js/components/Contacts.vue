<template>
    <div>
        <h1>Add Contacts</h1>
        <form action="#" @submit.prevent="edit ? updateContact(contact.id) : createContact()">
            <div class="form-group">
                <label for="name">Name</label>
                <input v-model="contact.name" type="text" class="form-control" name="name">
            </div>
            <div class="form-group">
                <label for="email">Email</label>
                <input v-model="contact.email" type="text" class="form-control" name="email">
            </div>
            <div class="form-group">
                <label for="phone">Phone</label>
                <input v-model="contact.phone" type="text" class="form-control" name="phone">
            </div>
            <div class="form-group">
                <button v-show="!edit" type="submit" name="button" class="btn btn-primary">New Contact</button>
                <button v-show="edit" type="submit" name="button" class="btn btn-primary">Update Contact</button>
            </div>
        </form>
        <h1>Contacts</h1>
        <ul class="list-group">
            <li v-for="contact in list" class="list-group-item">
                <strong>{{ contact.name }}</strong> {{ contact.email }} {{ contact.phone }}
                <button @click="showContact(contact.id)" class="btn btn-info btn-sm">Edit</button>
                <button @click="deleteContact(contact.id)" class="btn btn-danger btn-sm">Delete</button>
            </li>
        </ul>
    </div>
    
</template>

<script>
    export default {
        data: function(){
            return {
                edit: false,
                list: [],
                contact: {
                    id: '',
                    name: '',
                    email: '',
                    phone: ''
                }
            }
        },
        mounted: function(){
            console.log('Contacts components loaded....');
            this.fetchContactList();
        },
        methods: {
            fetchContactList: function(){
                console.log('Fetching contacts...');
                axios.get('api/contacts')
                    .then((response) => {
                        console.log(response.data);
                        this.list = response.data;
                    })
                    .catch((error) => {
                        console.log(error);
                    });
            },
            createContact: function(){
                console.log('Creating contact...');
                let self = this;
                let params = Object.assign({},self.contact);
                
                axios.post('api/contacts/store', params)
                    .then(function(){
                        self.contact.name = '';
                        self.contact.email = '';
                        self.contact.phone = '';
                        self.edit = false;
                        self.fetchContactList();
                    })
                    .catch(function(error){
                        console.log(error);
                    });

            },
            updateContact: function(id){
                console.log('Updating contact...'+id);

                let self = this;
                let params = Object.assign({},self.contact);
                
                axios.patch('api/contacts/' + id, params)
                    .then(function(){
                        self.contact.name = '';
                        self.contact.email = '';
                        self.contact.phone = '';
                        self.edit = false;
                        self.fetchContactList();
                    })
                    .catch(function(error){
                        console.log(error);
                    });

            },
            showContact: function(id){
                let self = this;
                axios.get('api/contacts/' + id)
                    .then(function(response) {
                        self.contact.id = response.data.id;
                        self.contact.name = response.data.name;
                        self.contact.email = response.data.email;
                        self.contact.phone = response.data.phone;
                    })
                self.edit = true;
            },
            deleteContact: function(id){
                console.log('Deleting contact..' + id);

                let self = this;

                axios.delete('api/contact/' + id)
                    .then(function(response){
                        self.fetchContactList();
                    })
                    .catch(function(error){
                        console.log(error);
                    })
            }
        }
    }
</script>