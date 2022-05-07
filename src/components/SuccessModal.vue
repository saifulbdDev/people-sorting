<template>
  <div class="modal">
    <div class="modal__content">
      <div class="modal__head">
        <p class="modal__head__title">Success</p>
        <svg
          @click="modals.success = false"
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
      <div class="modal__body">
        <p class="modal__body__description">Time required: {{ message }}</p>
        <p class="modal__body__description">Total moves: {{ move }}</p>
      </div>
      <div class="modal__footer">
        <button @click="modals.success = false" class="btn">Close</button>
        <button @click="restart" class="btn btn--primary">Restart</button>
      </div>
    </div>
  </div>
</template>
