<template>
  <div class="dropdown">
    <button class="dropdown-btn">Click Here</button>
    <ul class="dropdown-content">
      <li>Item 1</li>
      <li>Item 2</li>
      <li>Item 3</li>
    </ul>
  </div>
</template>

<script>
export default {};
</script>

<style>
.dropdown {
  position: relative;
  display: inline-block;
}

.dropdown-btn {
  background-color: #f1f1f1;
  border: none;
  color: black;
  padding: 8px 16px;
  font-size: 16px;
  cursor: pointer;
}

.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f9f9f9;
  min-width: 160px;
  box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
  padding: 12px 16px;
  z-index: 1;
}

.dropdown-content li {
  list-style-type: none;
  margin: 4px 0;
  cursor: pointer;
}

.dropdown:hover .dropdown-content {
  display: block;
}
</style>
