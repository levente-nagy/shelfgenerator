<style lang="scss">
#result-code {
  display:none;
}
.results-bttn{
  display: block;
  width: fit-content;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 2%;
}
.instructions-text{
  text-align: center;
  margin-top: 1%;
  margin-bottom: 1%;
}

</style>
<template>
  <div class="container">
    <div id="result-code">
      <svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" :viewBox="'0 0 ' + state.image.width + ' ' + state.image.height" preserveAspectRatio="xMinYMin meet" id="my_svg">
        <image :width="state.image.width" :height="state.image.height" :href="this.$shelfUrl"></image>
        <pattern id="imagePattern" patternUnits="objectBoundingBox"  width="1" height="1">
            <image href="//surveyfiles.dynata.com/emea/custom/tools/shelf/green_check.png" width="40" height="40" preserveAspectRatio="xMidYMid slice"></image>
        </pattern>
        <a class="exclude" data-fancybox="" :href="selection.url" v-for="selection in state.selections" :key="selection.url">
          <rect data-buy-state="false" style="fill-opacity:0" :x="selection.x" :y="selection.y" :width="selection.width" :height="selection.height" :target="selection.target"/>
        </a>
      </svg>
    </div>
    <codemirror :value="code" :options="cmOptions"></codemirror>
  </div>
</template>
<script>
import beautify from 'js-beautify'
import { codemirror } from 'vue-codemirror'
import 'codemirror/lib/codemirror.css'
import 'codemirror/theme/ambiance.css'
export default {
  name: 'ResultView',
  data () {
    return {
      instructions: false,
      state: this.$root.$data.state,
      code: '',
      shelfurl: '',
      cmOptions: {
        tabSize: 4,
        mode: 'text/html',
        theme: 'ambiance',
        lineNumbers: true,
        lineWrapping: true,
        line: true
      }
    }
  },
  mounted () {
    const rawCode = document.getElementById('result-code').innerHTML
    this.code = beautify.html(rawCode, {
      indent_size: 2
    }).replace(/<a/g, '\n  <a')
  },
  components: {
    codemirror
  }
}
</script>
