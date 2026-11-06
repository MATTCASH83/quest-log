<script setup></script>

<template>
  <Teleport to="body">
    <div class="modal-overlay">
      <dialog open :class="['modal-content', 'dialog-box']">
        <header class="modal-header">
          <slot name="header">Default Title</slot>
        </header>

        <main class="modal-body">
          <slot></slot>
        </main>
      </dialog>
    </div>
  </Teleport>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(82, 82, 82, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}
.modal-header {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.25rem;
  font-weight: bold;
  margin-bottom: 20px;
  color: rgb(243, 125, 70);
}
</style>
