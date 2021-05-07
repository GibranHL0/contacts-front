<template>
  <div key="main">
    <div key="content" class="grid justify-center">
      <div key="title">
        <h2 class=" flex text-4xl justify-center pt-20 text-white font-bold">
          Welcome to Gibran Contacts APP
        </h2>
      </div>
    </div>
    <Table/>
  </div>
</template>

<script>
import Table from './components/Table.vue'

export default {
  name: 'App',
  components: {
    Table
  }
}
</script>
