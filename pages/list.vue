<template>
  <v-container fluid>
    <v-card>
      <v-toolbar color="success" dark flat>
        <v-toolbar-title>list</v-toolbar-title>
        <v-spacer />
        <v-sheet width="100" color="transparent">
          <v-select
            v-model="sortName"
            :items="['date', 'title']"
            hide-details
            solo-inverted
            flat
          />
        </v-sheet>
        <v-btn @click="list" icon>
          <v-icon>mdi-refresh</v-icon>
        </v-btn>
      </v-toolbar>
      <v-card-text>
        <template v-for="(item, i) in items">
          <v-list-item :key="item.id" three-line>
            <v-list-item-content>
              <v-list-item-title>{{ item.title }}</v-list-item-title>
              <v-list-item-subtitle>{{ item.description }}</v-list-item-subtitle>
              <v-list-item-subtitle>{{ item.date }}</v-list-item-subtitle>
            </v-list-item-content>
            <v-list-item-action>
              <v-btn :to="`/doc/${item.category}/${item.name}`" icon>
                <v-icon>mdi-arrow-right</v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item>
          <v-divider :key="i" />
        </template>
      </v-card-text>
      <v-card-actions>
        <v-btn @click="next" text>
          more
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-container>
</template>
<script>
export default {
  data () {
    return {
      items: [],
      last: null,
      sortName: 'date'
    }
  },
  watch: {
    sortName () {
      this.list()
    }
  },
  mounted () {
    this.list()
  },
  methods: {
    async list () {
      this.items = []
      const sn = await this.$fireStore.collection('docs')
        .orderBy(this.sortName, 'desc')
        .limit(3)
        .get()
      sn.docs.forEach((v) => {
        const item = v.data()
        item.id = v.id
        item.category = v.id.split('-')[0]
        item.name = v.id.split('-')[1]
        this.items.push(item)
      })
      this.last = sn.docs[sn.docs.length - 1]
    },
    async next () {
      const sn = await this.$fireStore.collection('docs')
        .orderBy(this.sortName, 'desc')
        .startAfter(this.last)
        .limit(3)
        .get()
      sn.docs.forEach((v) => {
        const item = v.data()
        item.id = v.id
        item.category = v.id.split('-')[0]
        item.name = v.id.split('-')[1]
        this.items.push(item)
      })
      this.last = sn.docs[sn.docs.length - 1]
    }
  }
}
</script>
