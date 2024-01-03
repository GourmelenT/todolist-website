<template>
  <select @change="changeStatus">
    <option value="A faire">A faire</option>
    <option value="En cours">En cours</option>
  </select>
</template>

<script>
export default {
  name: 'ChangeStatus',
  props: ['index', 'updateStatus'],
  methods: {
    changeStatus(event) {
      const value = event.currentTarget.value;
      this.updateStatus(this.index, value);
    }
  }
} 
</script>