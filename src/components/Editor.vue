<template>
  <div ref="editor">
    <!-- {{msg}} -->
  </div>
</template>

<script>
import "./style.css";
import { defaultValueCtx, Editor, rootCtx } from "@milkdown/core";
import {
  collaborative,
  CollabService,
  collabServiceCtx,
} from "@milkdown/plugin-collaborative";
import { math } from "@milkdown/plugin-math";
import { gfm } from "@milkdown/preset-gfm";
import { nord } from "@milkdown/theme-nord";
import { WebsocketProvider } from "y-websocket";
import { Doc } from "yjs";

import { tooltip } from "@milkdown/plugin-tooltip";
import { listener } from "@milkdown/plugin-listener";
import { prism } from "@milkdown/plugin-prism";
import { clipboard } from "@milkdown/plugin-clipboard";
import { menu } from "@milkdown/plugin-menu";
import { block } from "@milkdown/plugin-block";
import { cursor } from "@milkdown/plugin-cursor";
import { diagram } from "@milkdown/plugin-diagram";
import { emoji } from "@milkdown/plugin-emoji";
import { history } from "@milkdown/plugin-history";
import { indent } from "@milkdown/plugin-indent";
import { slash } from "@milkdown/plugin-slash";
import { trailing } from "@milkdown/plugin-trailing";
import { upload } from "@milkdown/plugin-upload";
const markdown = `
# Milkdown Collaborative Example

---

Now you can play!
`;

export default {
  name: "Editor",
  props: ["msg"],
  data() {
    return {
      room: "milkdown",
      doc: null,
      wsProvider: null,
    };
  },

  mounted() {
    const options = [
      { color: "#5e81AC", name: "milkdown user 1" },
      { color: "#8FBCBB", name: "milkdown user 2" },
      { color: "#dbfdbf", name: "milkdown user 3" },
      { color: "#D08770", name: "milkdown user 4" },
    ];
    const rndInt = Math.floor(Math.random() * 4);

    const status$ = document.getElementById("status");
    const connect$ = document.getElementById("connect");
    const disconnect$ = document.getElementById("disconnect");

    const apply$ = document.getElementById("apply");
    const template$ = document.getElementById("template");

    const room$ = document.getElementById("room");
    const toggle$ = document.getElementById("toggle");

    const autoConnect = true;

    const doc = new Doc();
    class CollabManager {
      constructor(collabService) {
        if (room$) {
          room$.textContent = this.room;
        }
      }

      
    }

    

    this.main();
  },
  methods: {
    async main() {
      const editor = await Editor.make()
        .config((ctx) => {
          ctx.set(rootCtx, document.getElementById("app"));
          ctx.set(defaultValueCtx, markdown);
        })
        .use(nord)
        .use(gfm)
        .use(math)
        .use(collaborative)
        .use(tooltip)
        .use(listener)
        .use(prism)
        .use(clipboard)
        .use(menu)
        .use(block)
        .use(cursor)
        .use(diagram)
        .use(emoji)
        .use(history)
        .use(indent)
        .use(slash)
        .use(trailing)
        .use(upload)
        .create();

      editor.action((ctx) => {
        const collabService = ctx.get(collabServiceCtx);
        const collabManager = new CollabManager(collabService);
        collabManager.flush(markdown);

        if (connect$) {
          connect$.onclick = () => {
            collabManager.connect();
          };
        }

        if (disconnect$) {
          disconnect$.onclick = () => {
            collabManager.disconnect();
          };
        }

        if (apply$ && template$) {
          apply$.onclick = () => {
            if (template$ instanceof HTMLTextAreaElement) {
              collabManager.applyTemplate(template$.value);
            }
          };
        }

        if (toggle$) {
          toggle$.onclick = () => {
            collabManager.toggleRoom();
          };
        }
      });

      return editor;
    },
    flush(template) {
        this.doc?.destroy();
        this.wsProvider?.destroy();

        this.doc = new Doc();
        this.wsProvider = new WebsocketProvider(
          "ws://127.0.0.1:12345",
          this.room,
          this.doc,
          { connect: autoConnect }
        );
        this.wsProvider.awareness.setLocalStateField("user", options[rndInt]);
        this.wsProvider.on("status", (payload) => {
          if (status$) {
            status$.innerText = payload.status;
          }
        });

        this.collabService
          .bindDoc(this.doc)
          .setAwareness(this.wsProvider.awareness);
        this.wsProvider.once("synced", async (isSynced) => {
          if (isSynced) {
            this.collabService.applyTemplate(template).connect();
          }
        });
      },

      connect() {
        this.wsProvider.connect();
        this.collabService.connect();
      },

      disconnect() {
        this.collabService.disconnect();
        this.wsProvider.disconnect();
      },

      applyTemplate(template) {
        this.collabService
          .disconnect()
          .applyTemplate(template, () => true)
          .connect();
      },

      toggleRoom() {
        this.room = this.room === "milkdown" ? "sandbox" : "milkdown";
        if (room$) {
          room$.textContent = this.room;
        }

        const template = this.room === "milkdown" ? markdown : "# Sandbox Room";
        this.disconnect();
        this.flush(template);
      }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
