<template>
  <div class="modal">
    <div class="modal__content">
      <div class="modal__head">
        <p class="modal__head__title">Success</p>
        @click="close"
        <img
          width="18"
          height="19"
          class="modal__head__close"
          @click="close"
          src="@/images/close.svg"
        />
      </div>
      <div class="modal__body">
        <p class="modal__body__description">Time required: {{ message }}</p>
        <p class="modal__body__description">Total moves: {{ move }}</p>
      </div>
      <div class="modal__footer">
        <button @click="close" class="btn">Close</button>
        <button @click="$emit('restart')" class="btn btn--primary">
          Restart
        </button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "SortableModal",
  props: {
    message: {
      type: String,
      default: "",
    },
    move: {
      type: Number,
    },
  },

  methods: {
    close() {
      this.$emit("close");
    },
  },
};
</script>

<style lang="sass" scoped>
@import '../styles/_modal.scss';
</style>
