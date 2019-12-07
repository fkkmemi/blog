<template>
  <v-container fluid>
    <v-card>
      <v-toolbar dense color="success" dark flat>
        <v-toolbar-title>write</v-toolbar-title>
        <v-spacer />
        <v-btn @click="save" icon>
          <v-icon>mdi-content-save</v-icon>
        </v-btn>
      </v-toolbar>
      <v-form>
        <v-card-text>
          <v-text-field v-model="docId.category" label="category" />
          <v-text-field v-model="docId.name" label="name" />
          <v-divider />
          <v-text-field v-model="doc.title" label="title" />
          <v-text-field v-model="doc.date" label="date" />
          <v-text-field v-model="doc.description" label="description" />
          <v-divider />
          <v-textarea v-model="content.text" label="content" />
        </v-card-text>
      </v-form>
    </v-card>
  </v-container>
</template>
<script>
export default {
  data () {
    return {
      docId: {
        category: '',
        name: ''
      },
      doc: {
        createdAt: new Date(),
        title: '',
        date: '',
        description: '',
        tags: []
      },
      content: {
        createdAt: new Date(),
        modifiedAt: new Date(),
        text: ''
      }
    }
  },
  methods: {
    async save () {
      const id = this.docId.category + '-' + this.docId.name
      this.doc.createdAt = new Date()
      await this.$fireStore.collection('docs').doc(id).set(this.doc)
      const cid = id + '/content/last'
      this.content.createdAt = new Date()
      this.content.modifiedAt = new Date()
      await this.$fireStore.collection('docs').doc(cid).set(this.content)
    }
  }
}
</script>
