<template>
    <div>
        <form action="" method="post">
            <label for="firstname">First Name
                <input type="text" name="firstname" id="firstname" v-model="firstname">
              <!--  <div class="validation"><b>Here</b></div>-->
                <validation-message v-if="validationMessageForAnagraphicsIsVisible" :message="contactNameSurnameAlreadyPresent"></validation-message>
            </label><br>
            <label for="lastname">Last Name
                <input type="text" name="lastname" id="lastname" v-model="lastname">
                <validation-message v-if="validationMessageForAnagraphicsIsVisible" :message="contactNameSurnameAlreadyPresent"></validation-message>
            </label><br>
            <label for="age"> Age
                <input type="text" name="age" id="age" v-model="age">
                <validation-message v-if="validationMessageForAgeIsVisible" :message="contactAgeIsNotANumber"></validation-message>
            </label><br>
            <button type="button" @click="processPost" name="new_contact_button">Send!</button>
        </form>
    </div>
</template>

<script>
    import axios from 'axios';
    import router from "../router";
    import ValidationMessage from './ValidationMessage'

    export default {
        name: "InsertContact",
        components: {
            ValidationMessage
        },
        methods: {
            processPost: function(){



                const nameNotExists = this.nameNotExists() ;
                const ageIsNumberOK  = this.ageIsNumber();

                if (nameNotExists && ageIsNumberOK){
                    console.log(this.contacts,this.firstname,this.lastname);
                 /*   axios.post('http://localhost:3000/contacts',
                        {'first_name':this.firstname,
                            'last_name': this.lastname,
                            'age': this.age
                        }
                    ).then(response =>router.push('/'));*/
                }

                if (!nameNotExists){
                    this.validationMessageForAnagraphicsIsVisible = true;
                }
                if (!ageIsNumberOK){
                    this.validationMessageForAgeIsVisible = true;
                }


            },
            nameNotExists: function(){
                let findByNameAndSuremameLambda = (element) => element.first_name == this.firstname && element.last_name == this.lastname;
                const index = this.contacts.findIndex(findByNameAndSuremameLambda);
                return index === -1;
            },
            ageIsNumber: function(){
                this.age = parseInt(this.age);
                return !isNaN(this.age) && this.age != undefined && this.age != null;
            }
        },
        data: function () {
            return {
                firstname:'',
                lastname:'',
                age:'',
                contacts : [],
                contactNameSurnameAlreadyPresent : 'Looks like contact is already in store!',
                contactAgeIsNotANumber :'Looks like age is not a number!',
                validationMessageForAnagraphicsIsVisible : false,
                validationMessageForAgeIsVisible : false,
            }
        },
        mounted : function () {
            axios.get('http://localhost:3000/contacts')
                .then(response => this.contacts = response.data)
                .catch(()=> this.netWorkingError = true);
        }




    }
</script>
<style>
    .validation {

        color: darkred;
        font-size: small;
        font-style: italic;
    }


</style>