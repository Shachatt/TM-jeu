<script setup lang="ts">

</script>

<template>
  <div class="plateau">
    <div class="ligne">
      <div class="case">?</div>
      <div class="case">?</div>
      <div class="case">?</div>
    </div>
    <div class="ligne">
      <div class="case">?</div>
      <div class="case">?</div>
      <div class="case">?</div>
    </div>
    <div class="ligne">
      <div class="case">?</div>
      <div class="case">?</div>
      <div class="case">?</div>
    </div>
  </div>
</template>

<style scoped>
.plateau {
  display: flex;
  flex-direction: column;
  gap: 5px;
  background-color: #e0f7fa;
  padding: 20px;
  border-radius: 10px;
}

.ligne {
  display: flex;
  gap: 5px;
}

.case {
  width: 100px;
  height: 100px;
  background-color: white;
  border: 2px solid #333;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 40px;
  font-weight: bold;
  cursor: pointer;
}
</style>
