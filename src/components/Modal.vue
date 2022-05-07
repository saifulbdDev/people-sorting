<template>
  <div class="modal">
    <div class="modal__content">
      <div class="modal__head">
        <p class="modal__head__title">How many people?</p>
        <img
          width="18"
          height="19"
          class="modal__head__close"
          @click="close"
          src="@/images/close.svg"
        />
      </div>
      <div
        class="modal__body"
        :class="{
          error__body: value < 20 || value > 100,
        }"
      >
        <p class="modal__body__description">
          Enter a number of how many people you want to add to the list.
        </p>
        <input
          class="modal__body__input"
          type="number"
          min="20"
          :value="value"
          @input="$emit('input', $event.target.value)"
          max="100"
        />
        <p v-if="value < 20 || value > 100" class="error-msg">
          Please enter a valid range between 20 to 100!
        </p>
      </div>
      <div class="modal__footer">
        <button @click="close" class="btn">Cancel</button>
        <button
          @click="$emit('start')"
          :class="{
            'btn--disabled': value < 20 || value > 100,
          }"
          class="btn btn--primary"
        >
          Start
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SortableModal",
  props: {
    value: {
      type: [String, Number],
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
