<template>
  <div>
    <!-- YourViewer component keeps visible in all pages using this layout -->
    <Navbar />
    <!-- Every page is loaded inside nuxt tag -->
    <nuxt />
  </div>
</template>

<script>
import Navbar from '../components/Navbar'
export default {
  name: 'RootItem',
  components: {
    Navbar
  }
}
</script>
