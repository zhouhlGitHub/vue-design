<template>
  <section class="view">
    <codemirror ref="cm" :value="code" :options="cmOptions"/>
  </section>
</template>

<script>
import {codemirror} from 'vue-codemirror'
import pretty from 'pretty'
import {mapGetters} from 'vuex'
import {stringifyTemplate} from '@/utils/component'
import 'codemirror/lib/codemirror.css'
export default {
  components: {
    codemirror
  },
  data () {
    return {
      cmOptions: {
        tabSize: 4,
        theme: 'base16-dark',
        readOnly: true,
        lineNumbers: true,
        line: true
      }
    }
  },
  computed: {
    ...mapGetters(['components']),
    code () {
      return pretty(`${this.template}${this.scripts}${this.styles}`)
    },
    template () {
      return `<template>${stringifyTemplate(this.components)}</template>`
    },
    scripts () {
      /* eslint-disable */
      return `<script>export default {}<\/script>`
    },
    styles () {
      return `<style>.view {}</style>`
    }
  },
  activated () {
    this.$refs['cm'].codemirror.refresh()
  }
}
</script>

<style lang="scss">
.CodeMirror {
  height: auto;
}
</style>

