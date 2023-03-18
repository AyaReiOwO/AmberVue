<template>

  <div>
    <div v-if="isLoading" class="loading gradient"></div>
    <div v-else-if="error">Couldn't load the image :(</div>
    <img v-else :src="url" />
  </div>

</template>

<script setup>


// Change this to a non-existent URL to see the error state
const url = 'https://img2.baidu.com/it/u=3489701467,2838969942&fm=253&fmt=auto&app=138&f=JPEG?w=550&h=366'
const { isLoading, error } = useImage(
    {
      src: url,
    },
    {
      // Just to show the loading effect more clearly
      delay: 2000,
    }
)

</script>

<style scoped>

</style>