<template>
  <div class="modal">
    <div class="modal__content">
      <div class="modal__head">
        <p class="modal__head__title">How many people?</p>
        <svg
          @click="close"
          class="modal__head__close"
          width="18"
          height="19"
          viewBox="0 0 18 19"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M11.4443 9.82843L18 3.27277L15.5557 0.82843L9 7.3841L2.44434 0.828433L0 3.27277L6.55566 9.82843L4.43441e-06 16.3841L2.44434 18.8284L9 12.2728L15.5557 18.8284L18 16.3841L11.4443 9.82843Z"
            fill="#999999"
          />
        </svg>
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
    modal: {
      type: Boolean,
      default: false,
    },
    value: {
      type: Number,
      default: 20,
    },
  },
  model: {
    set(value) {
      this.$emit("input", value);
    },
    get() {
      return this.value;
    },
  },
  methods: {
    close() {
      this.$emit("close");
    },
  },
};
</script>

<style></style>
