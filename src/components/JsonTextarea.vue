<template>
  <div class="shadow border">
    <textarea :id="id" v-bind:value="value" />
  </div>
</template>

<script>
import CodeMirror from "codemirror";
import "codemirror/mode/javascript/javascript.js";

export default {
  name: "JsonTextarea",
  props: {
    id: { type: String },
    value: { type: String }
  },
  data() {
    return {
      editor: null,
      contentBackup: this.value
    };
  },
  watch: {
    value: function(newV) {
      if (this.contentBackup !== newV) {
        this.editor.setValue(newV, 1);
        this.contentBackup = newV;
      }
    }
  },
  mounted() {
    this.editor = CodeMirror.fromTextArea(document.getElementById(this.id), {
      value: this.value,
      lineNumbers: true
    });
    this.editor.on("change", cm => {
      this.contentBackup = cm.getValue();
      this.$emit("input", this.contentBackup);
    });
  }
};
</script>

<style>
.CodeMirror {
  height: 100px;
}
</style>
