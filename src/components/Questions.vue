<template>
  <div>
    <h3 v-if="!started">ARE YOU READY?</h3>
    <div class="backpack-image1 animated slideInUp" v-if="isActive">
      <img :src="allTheBackpacks.image" alt="backpack-image" class="backpack-image"  />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    allTheBackpacks: Object,
    isActive: Boolean,
    started: Boolean
  }
}
</script>