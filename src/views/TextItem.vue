<template>
  <div class="block__inner">
        <div class="text__item">
          <p @mouseup="highlight" :style="{ fontSize:selected + 'px'}">{{ text }}</p>
        </div>
        <div class="text__btns">
          <div class="colors__select">
            <p>Выберите цвет текста:</p>
          <label v-for="color in colors">
            <input type="radio" v-model="currentColor" :value="color">
            {{color}}
          </label>
          </div>
          <div class="select__btn">

          <select v-model="selected">
            <option disabled value="">Выберите один из вариантов</option>
            <option v-for="size in sizeText" >{{ size }}</option>
          </select>
          <span>Выбрано: {{ selected }}</span>
<!--          <button @click="openTextArea">Edit</button>-->
<!--          <button @click="editText">save</button>-->
          </div>
          <button @click="deleteText">delete</button>
        </div>

  </div>
</template>

<script>

export default {
  name: "TextItem",
  props: {
    text: {
      type: Object | String
    }
  },
	data() {
    return {
      isEdit: false,
      currentText: this.text.content,
      currentColor:'#ccc',
      colors: ["#ccc","#ff0000","#00ff00","#0000ff"],
      selectedRange:null,
      selected:'',
      sizeText:[12,14,16,18,20,22,36]
    }
	},
  // @message-saved="handleMessage"
  methods: {
    openTextArea() {
      this.isEdit = !this.isEdit;
    },
    deleteText() {
      this.$emit("deleteText", this.id);
    },
    editText() {
      this.$emit("editText", this.currentText, this.text.id);
    },
    getSelectedText() {
      if (window.getSelection) {
        let sel = window.getSelection()
        if (sel.getRangeAt && sel.rangeCount) {
          this.selectedRange = sel.getRangeAt(0);
        }
        return window.getSelection().toString()
      } else if (document.selection) {
        this.selectedRange = document.selection.createRange()
        return document.selection.createRange().text
      }
      return ''
    },
    surroundSelection(color) {
      const span = document.createElement("span");
      span.className = 'highlight'
      span.style.fontWeight = "bold"
      span.style.color = color
      span.addEventListener("click", () => {
        console.log('click');
      });
      if (window.getSelection) {
        const sel = window.getSelection()
        if (sel.rangeCount) {
          const range = this.selectedRange.cloneRange()
          range.surroundContents(span)
          sel.removeAllRanges()
          sel.addRange(range)
        }
      }
    },
    highlight(){
      let text = this.getSelectedText();
      if (this.selectedRange && text) {
        this.surroundSelection(this.currentColor)
      }
    }

  }
}
</script>
<style lang="scss">
.text__selection{
  margin: 0 auto;
  width: 900px;
  border: 2px solid skyblue;
}
.block__inner{
  max-width: 1400px;
  min-height: 150px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border:1px solid green;
  margin: 50px auto;
}
.text__item{
  width: 60%;

 }
.text__btns{
  width: 40%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.colors__select{
  width: 250px;
}

</style>
