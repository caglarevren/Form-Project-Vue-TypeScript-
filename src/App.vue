<template>
  <div>
    <SignupForm />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import SignupForm from '@/components/SignupForm.vue'

@Component({
  components: {
    SignupForm,
  },
})
export default class App extends Vue {}
</script>

<style lang="scss"></style>
