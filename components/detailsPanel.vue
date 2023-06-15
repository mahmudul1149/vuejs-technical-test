<template>
  <div class="details-view">
    <div class="text-center">
      <p class="title">View Details</p>
      <span>--------------------------------------------</span>
    </div>
    <p>Name: {{ item.title }}</p>
    <p>Price: {{ item.price }}</p>
    <p>Rating: {{ item.rating }}</p>
    <p>Description: {{ item.description }}</p>
  </div>
</template>

<script>
export default {
  name: "DetailsView",
  props: {
    item: Object,
  },
};
</script>

<style scoped>
.text-center {
  text-align: center;
}
.details-view {
  background-color: #ddd;
  padding: 20px;
  color: black;
}
.details-view .title {
  font-family: "Roboto", sans-serif;
  font-size: 1rem;
  font-weight: 600;
  opacity: 0.6;
}
.details-view p {
  font-family: "Roboto", sans-serif;
  font-size: 1.1rem;
  color: black;
  opacity: 0.8;
}
</style>
