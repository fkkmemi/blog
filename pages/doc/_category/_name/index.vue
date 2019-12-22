<template>
  <v-container fluid>
    <v-card>
      <v-card-title>
        {{ meta.title }}
        <v-spacer />
        <v-btn @click="$router.push('/list')" icon>
          <v-icon>mdi-arrow-left</v-icon>
        </v-btn>
      </v-card-title>
      <v-subheader>{{ meta.description }} / {{ meta.date }}</v-subheader>
      <v-divider />
      <v-card-text v-html="$md.render(doc.text)" />
    </v-card>
  </v-container>
</template>
<script>

export default {
  data () {
    return {
      meta: {
        title: '',
        description: ''
      },
      doc: {
        text: ''
      }
    }
  },
  async mounted () {
    await this.metaRead()
    await this.docRead()
  },
  methods: {
    async metaRead () {
      const id = this.$route.params.category + '-' + this.$route.params.name
      const sn = await this.$fireStore.collection('docs').doc(id).get()
      this.meta = sn.data()
    },
    async docRead () {
      const id = this.$route.params.category + '-' + this.$route.params.name + '/content/last'
      const sn = await this.$fireStore.collection('docs').doc(id).get()
      this.doc = sn.data()
    }
  }
}
</script>
