<template>
    <div>
        <span v-if="netWorkingError" style="background-color: red; color: white"  v-text="netWorkingErrorMessage"></span>
        <table>
            <tr>
                <th>Firstname</th>
                <th>Lastname</th>
                <th>Age</th>
            </tr>
            <tr v-for="contact in contacts" :key="contact.id">
                <td v-text="contact.first_name"></td>
                <td v-text="contact.last_name"></td>
                <td v-text="contact.age"></td>
            </tr>
        </table>
    </div>
</template>

<script>
    import axios from 'axios';

    export default {
        name: "ContactsList",

        data : function () {
            return {
                netWorkingError:false,
                contacts : [],
                netWorkingErrorMessage : 'Network problems'
            }
        },
        mounted: function () {
            axios.get('http://localhost:3000/contacts')
                .then(response => this.contacts = response.data)
                .catch(()=> this.netWorkingError = true)


        }
    }
</script>
<style>



</style>
