<template>
  <v-container fluid>
    <v-row>
      <v-toolbar dense>
        <v-btn @click="headingOne" text small title="Heading 1">
          <span class="font-weight-bold subtitle-1">H1</span>
        </v-btn>
        <v-btn @click="headingTwo" text small title="Heading 2">
          <span class="font-weight-bold subtitle-1">H2</span>
        </v-btn>
        <v-btn @click="boldText" text small title="Bold">
          <span class="font-weight-black subtitle-2">B</span>
        </v-btn>
        <v-btn @click="italicText" text small title="Italic">
          <span class="font-italic subtitle-2">I</span>
        </v-btn>
        <v-btn @click="insertLink" text small title="Insert Link">
          <v-icon class="title">mdi-link</v-icon>
        </v-btn>
        <v-btn @click="undoText" text small title="Undo">
          <v-icon class="title">mdi-undo</v-icon>
        </v-btn>
        <v-btn @click="redoText" text small title="Redo">
          <v-icon class="title">mdi-redo</v-icon>
        </v-btn>
        <v-btn @click="textLeft" text small title="Left Aligned">
          <v-icon class="title">mdi-format-align-left</v-icon>
        </v-btn>
        <v-btn @click="textCenter" text small title="Center Aligned">
          <v-icon class="title">mdi-format-align-center</v-icon>
        </v-btn>
        <v-btn @click="textRight" text small title="Right Aligned">
          <v-icon class="title">mdi-format-align-right</v-icon>
        </v-btn>
        <v-btn @click.stop="clearAll" text small title="Clear All">
          <v-icon class="title">mdi-format-clear</v-icon>
        </v-btn>
      </v-toolbar>
      <v-col style cols="12" id="text" @input="onUpdate" contenteditable="true"></v-col>
    </v-row>
    <v-dialog v-model="link.displayDialog" width="500">
      <v-card>
        <v-card-title>
          <span>Add a link</span>
        </v-card-title>
        <v-card-text>
          <v-text-field label="Link text" v-model="link.text" />
          <v-text-field label="Link URL" v-model="link.url" />
        </v-card-text>
        <v-card-actions>
          <div class="flex-grow-1"></div>
          <v-btn text @click="link.displayDialog = false" color="red" dark>cancel</v-btn>
          <v-btn text @click="addLink" color="primary" dark class="ml-5">add link</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  name: "v-rich-text-editor",
  data() {
    return {
      selectedText: "",
      enteredText: "",
      link: {
        displayDialog: false,
        text: "",
        url: "",
      },
      image: {
        displayDialog: false,
        alternativeText: "",
        url: "",
      },
      selection: {
        selectionObject: null,
        range: null,
      },
      display: {},
    };
  },

  watch: {
    selectedText: function (val) {
      // this.onUpdateFrom(val);
      // console.log(val);
      this.$emit("input", val);
    },
  },

  mounted() {
    document.getElementById("text").innerHTML = this.value;
  },

  methods: {
    clearData() {
      document.getElementById("text").innerHTML = "";
    },
    onUpdate(event) {
      this.selectedText = event.target.innerHTML;
      this.$emit("input", event.target.innerHTML);
    },
    headingOne: function () {
      document.execCommand("formatBlock", false, "h1");
    },
    headingTwo: function () {
      document.execCommand("formatBlock", false, "h2");
    },
    boldText: function () {
      document.execCommand("bold", false);
    },
    italicText: function () {
      document.execCommand("italic", false);
    },
    undoText: function () {
      document.execCommand("undo");
    },
    redoText: function () {
      document.execCommand("redo");
    },
    clearAll: function () {
      document.getElementById("text").innerHTML = "";
    },
    addLink: function () {
      this.link.displayDialog = false;

      let linkElement = document.createElement("a");
      linkElement.href = `http://${this.link.url}`;
      linkElement.text = this.link.text;
      linkElement.target = "_blank";

      this.selection.selectionObject.extend(
        this.selection.range.startContainer,
        this.selection.range.startOffset
      );
      this.selection.selectionObject.collapseToEnd();
      this.selection.range.insertNode(linkElement);
    },
    insertLink: function () {
      this.link.displayDialog = true;
      this.selection.selectionObject = document.getSelection();
      this.selection.range = this.selection.selectionObject.getRangeAt(0);
    },
    textLeft: function () {
      document.execCommand("justifyLeft");
    },
    textCenter: function () {
      document.execCommand("justifyCenter");
    },
    textRight: function () {
      document.execCommand("justifyRight");
    },
    onValid() {
      this.enteredText = this.selectedText;
      document.getElementById("text").innerHTML = "";
    },
  },
  props: ["value"],
};
</script>

<style scoped>
#text {
  outline: none;
  box-shadow: none;
}
#text:focus {
  outline: none;
  box-shadow: none;
}
</style>