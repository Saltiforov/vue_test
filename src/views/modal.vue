<template>
  <div class="modal-backdrop">
    <div class="modal">
      <header class="modal-header">
        <slot name="header">
              <p>Введите текст для редактирования</p>
          <button
              type="button"
              class="btn-close"
              @click="close"
          >
            x
          </button>
        </slot>
      </header>
      <section class="modal-body">
        <textarea class="text__area" placeholder="Введите текст для редактирования..." v-model="message"></textarea>
      </section>
          <button
              type="button"
              class="btn-green"
              @click="saveMessage"
          >
            Добавить текст на страницу!
          </button>

    </div>
  </div>
</template>

<script>
export default {
name: "modal",
  data() {
   return {
     showModal: true,
     message:'',
     messages:[],
   }
  },
  methods: {
    close(event) {
      this.$emit('close');
    },
    saveMessage(){
      console.log(typeof this.message)
      this.$store.commit('ADD_TEXT',this.message)
      console.log(this.$store.state.words)
      this.message = ''
    },

  }
}
</script>

<style lang="scss">
.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}
.text__area{
  resize: none;
  width: 400px;
  min-height: 150px;
  padding: 10px;
}
.modal {
  background: #FFFFFF;
  box-shadow: 2px 2px 20px 1px;
  overflow-x: auto;
  display: flex;
  flex-direction: column;
}
.modal-header{
  display: flex;
  align-items: center;
  p{
    padding-left: 20px;
  }
}

.modal-header {
  border-bottom: 1px solid #eeeeee;
  color: #4AAE9B;
  justify-content: space-between;
}

.modal-footer {
  border-top: 1px solid #eeeeee;
  justify-content: flex-end;
}

.modal-body {
  position: relative;
  padding: 20px 10px;
}

.btn-close {
  border: none;
  font-size: 20px;
  padding: 20px;
  cursor: pointer;
  font-weight: bold;
  color: #4AAE9B;
  background: transparent;
}

.btn-green {
  color: white;
  background: #4AAE9B;
  font-size: 22px;
  padding: 5px;
  border: 1px solid #4AAE9B;
  border-radius: 2px;
}
</style>
