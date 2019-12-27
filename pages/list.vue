<template>
  <v-container fluid>
    <v-card>
      <v-toolbar color="success" dark flat>
        <v-toolbar-title>list {{ items.length }}</v-toolbar-title>
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
      </v-toolbar>
      <v-card-text>
        <template v-for="(item, i) in items">
          <v-list-item :key="item.id" three-line>
            <v-list-item-content>
              <v-list-item-title>{{ i + ' ' + item.title }}</v-list-item-title>
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
      <v-card-actions v-intersect="onIntersect">
        <v-btn @click="next" :loading="loading" text>
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
      sortName: 'date',
      loading: false
    }
  },
  watch: {
    sortName () {
      this.items = []
      // this.next()
    }
  },
  mounted () {
  },
  methods: {
    async next () {
      const ref = this.$fireStore.collection('docs')
        .orderBy(this.sortName, 'desc')
        .limit(10)
      let sn
      this.loading = true
      try {
        if (!this.items.length) {
          sn = await ref.get()
        } else {
          if (!this.last) return
          sn = await ref.startAfter(this.last).get()
        }
        if (!sn.docs.length) {
          this.last = null
          return
        }
        sn.docs.forEach((v) => {
          const item = v.data()
          item.id = v.id
          item.category = v.id.split('-')[0]
          item.name = v.id.split('-')[1]
          this.items.push(item)
        })
        this.last = sn.docs[sn.docs.length - 1]
      } catch (e) {
        console.error(e.message)
      } finally {
        this.loading = false
      }
    },
    onIntersect (entries, observer, isIntersecting) {
      console.log(entries, observer, isIntersecting)
      if (isIntersecting) this.next()
    }
  }
}
</script>
