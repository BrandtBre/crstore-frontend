<template>
  
</template>

<script>
export default {
  name: "lougOut",
  layout: 'originalDefault',
  data () {return{}},

  created () {
    localStorage.removeItem('crstore-api-token');
    this.$route.push('/');
    this.$toast.success('O pai não ta mais on =o');
  }
}
</script>

