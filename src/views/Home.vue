<template>
  <v-container class="ma-0 pa-0" grid-list-sm>
    <div class="text-right">
      <v-btn small text to="/blogs" class="blue--text">
        All blogs <v-icon>mdi-chevron-right</v-icon>
      </v-btn>
    </div>
    <div class="text-left">
      <v-btn color="primary" class="mb-1">
        <v-icon left> mdi-book-plus-outline </v-icon>
        New Blog
      </v-btn>
    </div>

    <v-layout wrap>
      <blog-item-component 
      v-for="blog in blogs" 
      :key="`blog-`+blog.id"
      :blog="blog">
      </blog-item-component>  
    </v-layout>

    <button @click="increment(10)">Tambah</button>
    {{ tambah }}
  </v-container>
</template>

<script>
import BlogItemComponent from '../components/BlogItemComponent.vue'
import { mapMutations, mapGetters } from 'vuex'

export default {
  data: () => ({
    apiDomain: 'https://demo-api-vue.sanbercloud.com',
    blogs: [],
  }),

  components: {
    'blog-item-component': BlogItemComponent
  },

  computed: {
    // count() {
    //   return this.$store.getters.count
    // }

    ...mapGetters({
      'tambah' : 'counter/count'
    })
  },

  methods: {
    go() {
      const config = {
        method: 'get',
        url: this.apiDomain + '/api/v2/blog/random/4'
      }

      this.axios(config)
        .then(response => {
          let { blogs } = response.data
          this.blogs = blogs
        })
        .catch(error => {
          console.log(error)  
        })
    },
    
    // increment(payload) {
    //   this.$store.commit('increment', payload)
    // }

    ...mapMutations({
      'increment' : 'counter/increment'
    })
  },
  created() {
      this.go()
  }
}
</script>
