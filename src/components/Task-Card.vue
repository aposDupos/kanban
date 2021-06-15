<template>
  <div class="task-card">
    <Modal v-if="showModal"
           @closeModal="showModal=false"
           :priority="task.priority"
           :description="task.text"
           @submitModal="taskEdit($event)"
    >
      Изменить
    </Modal>
    <div class="task" draggable="true" @dragstart="dragStart($event, task)">
      <div class="task-title">
        <h3 class="task-title__heading">{{ "Задача №" + task.id }}</h3>
        <div
          class="task-title__priority"
          :class="'task-title__priority__' + priorityChanger"
        >
          {{ task.priority }}
        </div>
      </div>
      <div class="task-content">
        <p class="task-content__text">{{ task.text }}</p>
        <p class="task-content__date">{{ task.date }}</p>
      </div>
      <div class="task-buttons">
        <button class="task-button" :disabled="!column" @click.prevent="changeColumn('left')">
          <div class="task-button__icon arrow__left"><img src="../assets/arrow.png" alt=""></div>
        </button>
        <button class="task-button" @click="showModal = true">Edit</button>
        <button class="task-button" @click.prevent="changeColumn('right')">
          <div class="task-button__icon arrow__right"><img :src="buttonImage" alt="">
          </div>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import Modal from "@/components/Modal";

export default {
  name: "Task-Card",
  components: { Modal },
  data() {
    return {
      showModal: false
    };
  },
  props: {
    task: Object,
    column: Number
  },
  computed: {
    priorityChanger() {
      if (this.task.priority === 1) return "first";
      else if (this.task.priority === 2) return "second";
      else return "third";
    },
    buttonImage() {
      if (this.column !== 2) return require("../assets/arrow.png");
      else return require("../assets/close.png");
    }
  },
  methods: {
    changeColumn(direction) {
      this.$emit("column-change", {
        direction: direction,
        task: this.task
      });
    },
    dragStart(e, task) {
      e.dataTransfer.dropEffect = "move";
      e.dataTransfer.effectAllowed = "move";
      e.dataTransfer.setData("itemId", task.id.toString());
    },
    taskEdit(newTask) {
      this.task.text = newTask.description;
      this.task.priority = newTask.priority;
      this.showModal = !this.showModal;
    }
  }
};
</script>

<style scoped lang="scss">
.task {
  border: var(--task-color) 2px solid;
  background: var(--task-back);
  border-radius: 20px;
  padding: 10px;
  text-align: left;
  transition: 1s ease-in-out;
  cursor: move;
  margin-bottom: 1em;

  &-title {
    display: flex;
    justify-content: space-between;

    &__priority {
      width: 15px;
      height: 15px;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 30px;
      border: 2px grey solid;
      color: white;

      &__first {
        background: #f55c47;
      }

      &__second {
        background: #ffd56b;
      }

      &__third {
        background: #9fe6a0;
      }
    }
  }

  &-content {
    margin-top: 1em;

    &__text {
word-break: break-word;
    }

    &__date {
      margin-top: .5em;
    }
  }

  &-buttons {
    display: flex;
    justify-content: space-between;
    margin-top: .5em;
  }

  &-button {
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 10px 13px 29px -7px rgba(0, 0, 0, 0.37);
    border: 0;
    border-radius: 8px;

    &__icon {
      img {
        display: block;
        width: 20px;
        height: 20px;
      }
    }
  }
}

.arrow__left {
  transform: rotate(180deg);
}
</style>
