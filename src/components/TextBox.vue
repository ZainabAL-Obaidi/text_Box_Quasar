<template>
  <div>
    <q-input v-model="inputText" label="Enter text" @keyup.enter="addText" />
    <q-btn color="primary" @click="addText">Add</q-btn>

    <q-card v-if="submittedTextArray.length > 0" class="q-mt-md">
      <q-card-section>
        <q-card-title> Text entered: </q-card-title>
        <q-separator spaced />
        <div
          v-for="(text, index) in submittedTextArray"
          :key="index"
          class="text-entry"
        >
          <q-item v-if="editIndex !== index">
            <q-item-section>
              <q-item-label>{{ text }}</q-item-label>
            </q-item-section>
            <q-item-section side>
              <q-btn icon="edit" @click="startEdit(index)" />
              <q-btn
                icon="delete"
                @click="confirmDelete(index)"
                color="negative"
              />
            </q-item-section>
          </q-item>
          <q-item v-else>
            <q-input
              v-model="editedText"
              @keyup.enter="saveEdit(index)"
              @keyup.esc="cancelEdit"
            />
            <q-btn label="Save" color="primary" @click="saveEdit(index)" />
            <q-btn label="Cancel" color="negative" @click="cancelEdit" />
          </q-item>
        </div>
      </q-card-section>
    </q-card>

    <q-dialog v-model="dialogVisible">
      <q-card>
        <q-card-section>
          <q-card-title> Confirm Delete </q-card-title>
          Are you sure you want to delete "{{ deleteText }}"?
        </q-card-section>
        <q-card-actions align="right">
          <q-btn label="Cancel" color="primary" @click="cancelDelete" />
          <q-btn label="Delete" color="negative" @click="deleteConfirmed" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputText: "",
      submittedTextArray: [],
      editIndex: -1,
      editedText: "",
      dialogVisible: false,
      deleteIndex: -1,
    };
  },
  methods: {
    addText() {
      if (this.inputText.trim() !== "") {
        this.submittedTextArray.push(this.inputText);
        this.inputText = ""; // Clear the input after adding text
      }
    },
    startEdit(index) {
      this.editIndex = index;
      this.editedText = this.submittedTextArray[index];
    },
    saveEdit(index) {
      if (this.editedText.trim() !== "") {
        this.submittedTextArray[index] = this.editedText;
        this.cancelEdit();
      }
    },
    cancelEdit() {
      this.editIndex = -1;
      this.editedText = "";
    },
    confirmDelete(index) {
      this.deleteIndex = index;
      this.dialogVisible = true;
    },
    deleteConfirmed() {
      if (this.deleteIndex !== -1) {
        this.submittedTextArray.splice(this.deleteIndex, 1);
        this.dialogVisible = false;
        this.deleteIndex = -1;
      }
    },
    cancelDelete() {
      this.dialogVisible = false;
      this.deleteIndex = -1;
    },
  },
};
</script>

<style scoped>
.text-entry {
  margin-bottom: 10px;
}
</style>
