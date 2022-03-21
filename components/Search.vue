<template>
  <div>
    <form @submit="onSubmit">
      <input v-model="search_input" placeholder="Search movies" />
      <input type="submit" value="Search" />
    </form>
  </div>
</template>

<script>
export default {};
</script>

<style>
</style>