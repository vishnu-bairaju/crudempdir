<template>
  <Transition>
    <div class="modal">
      <div class="modal-container">
        <h1>Add/Edit user</h1>
        <div class="form-container">
          <div class="user-details">
            <slot name="fields"></slot>
          </div>
          <div class="btn-container" @click="$emit('close')">
            <button @click="$emit('close')">Close</button>
            <slot name="submit"></slot>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<style>
.modal {
  background-color: rgba(0, 0, 0, 0.3);
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: all 0.3s ease-in-out;
}

.modal-container {
  background-color: white;
  border-radius: 10px;
  min-width: 600px;
  min-height: 200px;
  max-width: 100%;
  padding: 30px;
}

h1 {
  text-align: center;
}

.user-details {
  padding: 20px;
}

.btn-container {
  display: flex;
  justify-content: space-around;
}

button {
  background-color: lightcoral;
  border-radius: 5px;
  border: 1px solid black;
  padding: 5px;
}
</style>
