<template>
  <div class="modal">
    <div class="modal-content">
      <button class="modal__close-button" @click="$emit('closeModal')">
        <img src="../assets/close.png" alt="" />
      </button>
      <form action="" class="modal__form">
        <label for="select">Приоритет
          <select name="priority" id="select" v-model="newTask.priority" class="modal__select">
            <option
              v-for="index in 3"
              :key="index"
              :selected="index === priority"
              :value="index"
            >
              {{ index }}
            </option>
          </select></label>
        <label for="description">Описание
          <input
            type="text"
            id="description"
            v-model.lazy="newTask.description"
            class="modal__description"
          /></label>
      </form>
      <button
        :disabled="!(newTask.description && newTask.priority)"
        class="btn"
        @click="
          $emit('submitModal', {
            priority: newTask.priority,
            description: newTask.description,
          })
        "
      >
        <slot></slot>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Modal",
  data() {
    return {
      index: 0,
      newTask: {
        description: "",
        priority: 0
      }
    };
  },
  mounted() {
    this.newTask.priority = this.priority || 0;
    this.newTask.description = this.description || "";
  },
  props: {
    priority: Number,
    description: String
  }
};
</script>

<style scoped lang="scss">
.modal {
  width: 100vw;
  height: 100vh;
  position: absolute;
  z-index: 10;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  top: 0;
  left: 0;

  &-content {
    background: var(--background-color);
    top: 50px;
    width: 400px;
    height: 200px;
    box-shadow: 10px 13px 29px -7px rgba(0, 0, 0, 0.37);
    border-radius: 32px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  &__form {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

  }

  &__description {
    outline: none;
    padding: 8px;
    border: 0;
    border-radius: 8px;
    background: #e5e5e5;
    box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2);
  }

  &__close-button {
    outline: none;
    background: none;
    border: 0;
    align-self: flex-end;
    margin: 1em 1em 0 0;

    img {
      width: 32px;
      height: 32px;
    }
  }

  &__select {
    margin-bottom: 20px;
    outline: none;
    padding: 3px;
    border-radius: 4px;
    border: 0;
    background: #e5e5e5;
    box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2);
  }
}
</style>
