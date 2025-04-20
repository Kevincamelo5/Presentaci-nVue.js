<template>
  <div class="app">
    <h1>WordCounter</h1>

    <div class="toolbar">
      <button @click="format('bold')">Negrita</button>
      <button @click="format('italic')">Cursiva</button>
      <button @click="format('underline')">Subrayado</button>
      <button @click="changeColor">Color</button>
    </div>

    <div
      class="editor"
      contenteditable="true"
      ref="editor"
      @input="updateText"
    ></div>

    <div class="info-box">
      <p><strong>Palabras:</strong> {{ wordCount }}</p>
      <p><strong>Caracteres:</strong> {{ characterCount }}</p>
      <p><strong>Resumen:</strong> {{ summary }}</p>
    </div>

    <div class="buttons">
      <button @click="clearText">Borrar texto</button>
      <button @click="exportPDF">Exportar a PDF</button>
    </div>
  </div>
</template>

<script>
import { jsPDF } from "jspdf";

export default {
  data() {
    return {
      text: "",
      currentColor: "#1e90ff"
    };
  },
  computed: {
    wordCount() {
      return this.text.trim().split(/\s+/).filter(w => w).length;
    },
    characterCount() {
      return this.text.length;
    },
    summary() {
  const words = this.text.trim().split(/\s+/);
  const maxWords = 30;

  if (words.length <= maxWords) {
    return this.text.trim();
  }

  const sliced = words.slice(0, maxWords).join(" ");
  return sliced + "...";
}
  },
  methods: {
    updateText() {
      this.text = this.$refs.editor.innerText;
    },
    clearText() {
      this.$refs.editor.innerHTML = "";
      this.text = "";
    },
    exportPDF() {
      const doc = new jsPDF();
      const content = this.$refs.editor.innerText;
      const splitText = doc.splitTextToSize(content, 180);
      doc.text("Tu texto exportado:", 10, 10);
      doc.text(splitText, 10, 20);
      doc.save("wordcouter.pdf");
    },
    format(command) {
      document.execCommand(command, false, null);
    },
    changeColor() {
      const colors = ["#1e90ff", "#e91e63", "#4caf50", "#ff9800", "#9c27b0"];
      this.currentColor =
        colors[(colors.indexOf(this.currentColor) + 1) % colors.length];
      document.execCommand("foreColor", false, this.currentColor);
    }
  }
};
</script>

<style>
.app {
  max-width: 700px;
  margin: 0 auto;
  padding: 2rem;
  font-family: 'Segoe UI', sans-serif;
  background: linear-gradient(to right, #c2e9fb, #a1c4fd);
  border-radius: 20px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #1e3c72;
  margin-bottom: 1rem;
}

.editor {
  min-height: 200px;
  background-color: white;
  padding: 1rem;
  font-size: 1rem;
  border-radius: 10px;
  border: none;
  outline: none;
  box-shadow: inset 0 0 5px #aaa;
  margin-bottom: 1rem;
  white-space: pre-wrap;
  overflow-wrap: break-word;
}

.toolbar {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 1rem;
  flex-wrap: wrap;
}

.toolbar button {
  padding: 0.5rem 0.9rem;
  background-color: #1e90ff;
  color: white;
  border: none;
  border-radius: 8px;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.3s ease;
}

.toolbar button:hover {
  background-color: #1565c0;
}

.info-box {
  background: #ffffffcc;
  padding: 1rem;
  border-radius: 10px;
  margin-bottom: 1rem;
  color: #333;
  font-size: 1rem;
}

.buttons {
  display: flex;
  justify-content: center;
  gap: 1rem;
}

button {
  padding: 0.7rem 1.2rem;
  border: none;
  border-radius: 10px;
  background-color: #1e90ff;
  color: white;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.3s ease;
  font-size: 1rem;
}

button:hover {
  background-color: #1565c0;
}
</style>
