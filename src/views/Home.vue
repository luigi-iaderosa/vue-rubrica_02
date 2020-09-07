<template>
  <div class="home">
    <HelloWorld msg="Contacts List"/>
    <div class>
      <ContactsList class="centered-element"></ContactsList>
    </div>

  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import ContactsList from '@/components/ContactsList'

export default {
  name: 'Home',
  components: {
    HelloWorld,ContactsList
  }
}
</script>
