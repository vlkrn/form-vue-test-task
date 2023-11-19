<template>
  <div v-if="isVisible" class="alert">{{ textMessage }}</div>
</template>

<script>
export default {
  name: 'TextAlert',
  props: ['textMessage', 'isVisible'],
};
</script>

<style scoped lang="scss">
.alert {
  position: fixed;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  background-color: #4caf50;
  color: #fff;
  padding: 15px;
  border-radius: 5px;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.2);
}
</style>
