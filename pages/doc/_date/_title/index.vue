<template>
  <v-card>{{ items }}</v-card>
</template>
<script>

export default {
  data () {
    return {
      items: []
    }
  },
  async mounted () {
    const sn = await this.$fireStore.collection('test').where('date', '==', this.$route.params.date).get()
    sn.docs.forEach((v) => {
      console.log(v.data())
      this.items.push(v.data())
    })
  }
}
</script>
