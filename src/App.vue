<template>
  <div key="main">
    <Welcome/>
    <Table/>
  </div>
</template>

<script>
import Welcome from './components/Welcome.vue'
import Table from './components/Table.vue'

export default {
  name: 'App',
  components: {
    Welcome,
    Table
  }
}
</script>
