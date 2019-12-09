<template>
    <div>
        <h1>Contacts</h1>
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
    </div>
</template>

<script>
    export default {
        data: function(){
            return {
                edit: true,
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
        },
        methods: {
            createContact: function(){
                console.log('Creating contact...');
            },
            updateContact: function(id){
                console.log('Updating contact...'+id);
            }
        }
    }
</script>