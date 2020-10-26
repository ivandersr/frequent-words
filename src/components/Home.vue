<template>
  <v-container fluid>
    <v-form>
      <v-file-input
        label="Selecione as legendas"
        prepend-icon="mdi-message-text"
        append-outer-icon="mdi-send"
        outlined
        multiple
        chips
        v-model="files"
        @click:append-outer="processSubtitles"
      />
    </v-form>
    <div class="pills">
      <Pill
        v-for="word in groupedWords"
        :key="word.name"
        :name="word.name"
        :amount="word.amount"
      />
    </div>
  </v-container>
</template>

<script>
import Pill from "./Pill";
const { ipcRenderer } = window.require("electron");

export default {
  components: { Pill },
  data: () => ({
    files: [],
    groupedWords: [],
  }),
  methods: {
    processSubtitles() {
      const paths = this.files.map((file) => file.path);

      ipcRenderer.send("process-subtitles", paths);
      ipcRenderer.on("process-subtitles", (event, response) => {
        this.groupedWords = response;
      });
    },
  },
};
</script>

<style>
.pills {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>