<template>
  <v-container>
    <v-list three-line>
      <v-list-item v-for="(item, i) in items" :key="i">
        <v-list-item-content>
          <v-list-item-title>{{ i + ' ' + item.title }}</v-list-item-title>
          <v-list-item-subtitle>{{ item.subtitle }}</v-list-item-subtitle>
          <v-list-item-subtitle>{{ item.date }}</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
    </v-list>
    <v-card v-intersect="onIntersect" class="text-center" flat>
      <v-btn :loading="loading">
        More
      </v-btn>
    </v-card>
  </v-container>
</template>
<script>
import delay from 'delay'

export default {
  data () {
    return {
      items: [],
      loading: false
    }
  },
  methods: {
    async onIntersect (entries, observer, isIntersecting) {
      // console.log(entries, observer, isIntersecting)
      this.loading = true
      await delay(3000)
      for (let i = 0; i < 10; i++) {
        this.items.push({
          title: Math.random(),
          subtitle: Math.random(),
          date: new Date().toLocaleString()
        })
      }
      this.loading = false
    }
  }
}
</script>
