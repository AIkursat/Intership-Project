<template>
  <transition name="fade">
    <div class="modal flex items-center justify-center" v-if="show">
      <div class="modal__backdrop" @click="closeModal()"/>

      <div class="modal__dialog">
        <div class="modal__header">
          <slot name="header"/>
          <button type="button" class="modal__close" @click="closeModal()">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 352 512">
              <path
                fill="currentColor"
                d="M242.72 256l100.07-100.07c12.28-12.28 12.28-32.19 0-44.48l-22.24-22.24c-12.28-12.28-32.19-12.28-44.48 0L176 189.28 75.93 89.21c-12.28-12.28-32.19-12.28-44.48 0L9.21 111.45c-12.28 12.28-12.28 32.19 0 44.48L109.28 256 9.21 356.07c-12.28 12.28-12.28 32.19 0 44.48l22.24 22.24c12.28 12.28 32.2 12.28 44.48 0L176 322.72l100.07 100.07c12.28 12.28 32.2 12.28 44.48 0l22.24-22.24c12.28-12.28 12.28-32.19 0-44.48L242.72 256z"
              ></path>
            </svg>
          </button>
        </div>

        <div class="modal__body">
          <slot name="body"/>
        </div>

        <div class="modal__footer">
          <slot name="footer"/>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: "Modal",
  props:{
    model:{
      type:Object,
      default:() => {}
    }
  },
  data() {
    return {
      show: false
    };
  },
  methods: {
    closeModal() {
      this.show = false;
      document.querySelector("body").classList.remove("overflow-hidden");
    },
    openModal() {
      this.show = true;
      document.querySelector("body").classList.add("overflow-hidden");
    }
  }
};
</script>

<style>
.modal{
    justify-content: center;
    position : fixed;
    left: 50%;
    top:45%;
}
.modal__backdrop{
  position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-color: rgba(0, 0, 0, 0.3);
    z-index: 1;
}
.modal__dialog{
     position: absolute;
    width: 600px;
    background-color: #ffffff;
    border-radius: 5px;
    margin: 50px auto;
    display: flex;
    flex-direction: column;
    z-index: 2;
}
.modal__close{
   width: 30px;
    height: 30px;
}
.modal__header{
  display: flex;
    align-items: flex-start;
    justify-content: space-between;
    padding: 20px 20px 10px;
}
.modal__body {
    padding: 10px 20px 10px;
    overflow: auto;
    display: flex;
    flex-direction: column;
    align-items: stretch;
  }
  .modal__footer {
    padding: 10px 20px 20px;
  }
  .fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
