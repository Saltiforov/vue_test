<template>
  <div class="home">
    <div class="text-item" >
      <button type="button" class="btn" @click="showModal">
        Open Modal!
      </button>
      <modal v-show="isModalVisible" @close="closeModal"/>
    </div>
    <template>
      <TextItem  v-for="text of getText" @editText="editText" @deleteText="deleteText" :key="text.id" :text="text" />
    </template>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
import TextItem from "./TextItem";
import { mapMutations } from "vuex";
import Modal from "@/views/modal";
export default {
  name: 'Home',
  data(){
    return {
      isModalVisible: true,
    }
  },
  components: {TextItem,Modal},
  computed: {
      ...mapGetters(["getText"])
  },
  methods: {
    ...mapMutations([
      "DELETE_TEXT",
      "EDIT_TEXT"
    ]),
    deleteText(id) {
      this.$store.commit("DELETE_TEXT", id);
    },
    editText(item, id) {
      this.$store.commit("EDIT_TEXT", { item, id });
    },
    showModal() {
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    }
  }
}
</script>
