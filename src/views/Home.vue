<template>
  <div class='content'>
    <div class="tabs">

      <div v-bind:class="{hide: isHide}" class='tabItem'>
        <textarea @input='onChange' v-model='markdown'># Markdown</textarea>
      </div>

      <div v-bind:class="{hide: !isHide}" class='tabItem'>
        <div class='preview markdown-body' v-html='html'></div>
      </div>

    </div>

    <div class='side'>
      <input type='button' @click="changeEditor" class='button' /><br>
      <button class='button' @click="addImage"></button>
    </div>

  </div>
</template>

<script>
const hljs = require('highlight.js')

const md = require('markdown-it')({
  langPrefix:'hljs language-',
  highlight: function (str, lang) {
    if (lang && hljs.getLanguage(lang)) {
      try {
        return hljs.highlight(str, { language: lang }).value;
      } catch (__) {console.error('MarkdownParse Error')}
    }
    return '';
  }
})

export default {
  name: 'Home',
  data() {
    return {
      text: '',
      markdown: '# This is Markdown Editor',
      html: '',
      isHide: false,
      imageTemplate: '![](https://upload.wikimedia.org/wikipedia/commons/9/95/Vue.js_Logo_2.svg)'
    }
  },
  methods: {
    onChange() {
      this.html = md.render(this.markdown)
    },
    changeEditor() {
      if (this.isHide) {
        this.isHide = false
      } else {
        this.isHide = true
      }
    },
    addImage() {
      this.markdown = this.markdown + `\n${this.imageTemplate}\n`
      this.html = md.render(this.markdown)
      
    }
  },
  mounted: function(){
    this.html = md.render(this.markdown)
  }
}
</script>

<style>
@import "../css/preview.css";
@import '../css/home_tabs.css';
@import url('https://fonts.googleapis.com/css2?family=Source+Code+Pro&display=swap');

textarea {
  margin: 0px;
  width: 100%;
  height: 500px;
  font-family: 'Source Code Pro', monospace;
  font-size: 16px;
}
</style>