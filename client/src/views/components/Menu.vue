<template lang="pug">
  el-menu(:default-active="$route.path")
    template(v-for="(item,key) in getPrivateRouter")
      el-submenu(:index="combinePath(item.path)" v-if="item.children && !item.hidden")
        template(slot="title")
          i.fl(v-if="item.meta.icon.indexOf('#') > -1")
            svg.icon.fs-20(aria-hidden="true")
              use(:xlink:href="item.meta.icon")
          i(v-else :class="item.meta.icon")
          span(slot="title") {{item.meta.title}}
        template(v-for="children in item.children")
          router-link(:to="combinePath(item.path,children.path)" :key="children.name" )
            el-menu-item(v-if="!children.hidden" :index="combinePath(item.path,children.path)") {{children.meta.title}}
</template>
<script>
import { mapGetters } from 'vuex'
import path from 'path'
export default {
  name: 'top',
  data () {
    return {
      self: this
    }
  },
  methods: {
    combinePath (a, b = '', c = '') {
      return path.resolve(a, b, c)
    }
  },
  computed: {
    ...mapGetters([
      'getPrivateRouter'
    ])
  }
}
</script>
<style scoped>
.el-menu{
  height: 100%;
}
</style>
