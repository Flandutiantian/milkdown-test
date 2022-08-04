<template>
  <div ref="editor"></div>
</template>

<script>
import { defaultValueCtx, Editor, rootCtx } from "@milkdown/core";
import { nord } from "@milkdown/theme-nord";
import { commonmark } from "@milkdown/preset-commonmark";
import { emoji } from "@milkdown/plugin-emoji";
export default {
  name: "Editor",
  props: {
    msg: String,
  },
  mounted() {
    Editor.make()
      .config((ctx) => {
        ctx.set(rootCtx, this.$refs.editor);
        ctx.set(defaultValueCtx, this.$props.msg);
      })
      .use(nord)
      .use(emoji)
      .use(commonmark)
      .create();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
