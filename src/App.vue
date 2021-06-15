<template>
  <div id="app" class="root" :data-theme="theme">
    <Modal v-if="showModal" @closeModal="showModal = false" @submitModal="createTask($event)">Создать</Modal>
    <header>
      <div class="container">
        <div class="header">
          <div class="header__logo">
            <img src="./assets/logo.png" alt="Logo" />
          </div>
          <h1 class="header__heading">Название проекта</h1>
          <div class="header__theme-switcher">
            <label for="theme"
            >{{ theme === "dark" ? "Темная" : "Светлая" }} тема</label
            >
            <input
              type="checkbox"
              id="theme"
              @change="theme = theme === 'light' ? 'dark' : 'light'"
            />

          </div>
        </div>
      </div>
    </header>
    <main>
      <div class="container">
        <div class="main">
          <button class="main__button btn" @click="showModal = true">Создать задачу</button>
          <div class="container">
            <div class="columns">
              <div
                class="column"
                v-for="(column, key) in columns"
                :key="key"
                @dragover.prevent="dragOver"
                @dragenter.prevent="dragEnter"
                @dragleave="dragLeave($event, key)"
                @drop="drop($event, key)"
              >
                <h2 class="column__heading">
                  {{ column.title + "(" + tasksCount(key) + ")" }}
                </h2>
                <div class="tasks">
                  <TaskCard
                    v-for="task in tasks.filter((task) => task.column === key)"
                    :key="task.id"
                    :task="task"
                    :column="key"
                    @column-change="changeColumn($event, task)"
                  />
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
    <footer>
      <div class="footer">
        <h3>Тыртычный В. А.</h3>
        <h3>Группа 201-322</h3>
      </div>
    </footer>
  </div>
</template>
<script>

import TaskCard from "@/components/Task-Card.vue";
import Modal from "@/components/Modal";

export default {
  name: "Home",
  components: { Modal, TaskCard },
  data() {
    return {
      lastId: 0,
      time: ((new Date).getDate() + "/" + ((new Date).getMonth() + 1)),
      showModal: false,
      columns: [
        {
          title: "План"
        },
        {
          title: "В работе"
        },
        {
          title: "Готово"
        }

      ],
      tasks: [
        {
          id: 1,
          date: `${String((new Date).getHours()).padStart(2, '0')}:${String((new Date).getMinutes()).padStart(2, '0')} ${(new Date).getDate()}/${String(((new Date).getMonth() + 1)).padStart(2, '0')}/${(new Date).getFullYear()}`,
          text: "QUUQQUUQUQUQUQUQU",
          priority: 1,
          column: 0
        },
        {
          id: 2,
          date: `${String((new Date).getHours()).padStart(2, '0')}:${String((new Date).getMinutes()).padStart(2, '0')} ${(new Date).getDate()}/${String(((new Date).getMonth() + 1)).padStart(2, '0')}/${(new Date).getFullYear()}`,
          text: "QUUQQUUQUQUQUQUQU",
          priority: 2,
          column: 1
        },
        {
          id: 3,
          date: `${String((new Date).getHours()).padStart(2, '0')}:${String((new Date).getMinutes()).padStart(2, '0')} ${(new Date).getDate()}/${String(((new Date).getMonth() + 1)).padStart(2, '0')}/${(new Date).getFullYear()}`,
          text: "QUUQQUUQUQUQUQUQU",
          priority: 3,
          column: 2
        },
        {
          id: 4,
          date: `${String((new Date).getHours()).padStart(2, '0')}:${String((new Date).getMinutes()).padStart(2, '0')} ${(new Date).getDate()}/${String(((new Date).getMonth() + 1)).padStart(2, '0')}/${(new Date).getFullYear()}`,
          text: "QUUQQUUQUQUQUQUQU",
          priority: 1,
          column: 0
        }

      ],
      finishedTasks: [],
      theme: "light"
    };
  },
  computed: {},
  methods: {
    tasksCount(id) {
      return this.tasks.filter((value, index) => {
        return value.column === id;
      }).length;
    },
    changeColumn(e, task) {
      if (e.direction === "right") {
        if (task.column < 2) {
          task.column++;
        } else {
          this.finishedTasks.push(task);
          this.tasks.splice(this.tasks.findIndex(el => el.id === task.id), 1);
        }
      } else task.column--;
    },

    dragEnd() {

    },
    dragOver() {

    },
    dragEnter() {

    },
    dragLeave() {
    },
    drop(e, column) {
      const itemId = parseInt(e.dataTransfer.getData("itemId"));
      this.tasks = this.tasks.map(value => {
        if (value.id === itemId)
          value.column = column;
        return value;
      });
    },
    createTask(task) {
      this.lastId += 1;
      let time = new Date;
      time = `${String(time.getHours()).padStart(2, '0')}:${String(time.getMinutes()).padStart(2, '0')} ${time.getDate()}/${String((time.getMonth() + 1)).padStart(2, '0')}/${time.getFullYear()}`;
      const newTask = {
        id: this.lastId,
        date: time,
        text: task.description,
        priority: task.priority,
        column: 0
      };
      this.tasks.push(newTask);
      this.showModal = false;
    }
  },
  mounted() {
    this.tasks.forEach(task => {
      if (task.id > this.lastId)
        this.lastId = task.id;
    });
  },
  created() {
    const time = new Date;
    this.time = time.getDate() + "/" + (time.getMonth() + 1);
  }
};
</script>
<style lang="scss">
@import "assets/colors";

#app {
  --main-color: #1687A7;
  --task-color: #D3E0EA;
  --background-color: #ffffff;
  --text-color: #000;
  --blue: #97def2;
  --task-back: #f0f0f0;
}

#app[data-theme = "dark"] {
  --main-color: #1A1A2E;
  --task-color: #D3E0EA;
  --background-color: #1d2c3c;
  --text-color: #fff;
  --blue: #346751;
  --task-back: #0F3460;
}


body,
* {
  margin: 0;
  padding: 0;
  transition: .5s ease-in-out;
}

h1, h2, h3, p, div, a {
  color: var(--text-color);
}

#app {
  display: grid;
  grid-template-rows: 100px 1fr 50px;
}

footer {
  background: var(--main-color);
}

.footer {
  border-radius: 32px 32px 0 0;
  background: var(--main-color);
}

.btn {
  padding: 8px;
  margin: 20px 0;
  border: var(--blue) 2px solid;
  border-radius: 8px;
  background: #fafafa;
}

.hovered {
  background: #9fe6a0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background: var(--background-color);
}

#app, main, header, body {
  background: var(--background-color);
}

#app {
  height: 100vh;
}

.container {
  max-width: 1140px;
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  margin: 0 auto;
}

header {
  background: var(--main-color);
  padding: 20px 0;
  border-radius: 0 0 32px 32px;
}

.header {
  grid-column: 1/-1;
  display: flex;
  align-items: center;
  justify-content: space-between;

  &__logo {
    display: flex;

    img {
      width: 48px;
      height: 100%;
    }
  }

  &__theme-switcher {
    display: flex;
    align-items: center;
  }
}

.main {
  grid-column: 1/-1;
  grid-auto-flow: column;

  .container {
  }
}

.columns {
  grid-column: 1/-1;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: auto;
  grid-auto-flow: column;
  column-gap: 30px;
  justify-content: space-between;
  flex-wrap: wrap;
  @media (max-width: 890px) {
    display: flex;
    justify-content: space-evenly;
  }

  .column {
    background: var(--blue);
    padding: 20px;
    box-shadow: 10px 13px 29px -7px rgba(0, 0, 0, 0.37);
    border-radius: 32px;
    @media (max-width: 890px) {
      margin-bottom: 1em;
    }

    &__heading {
      margin-bottom: 0.5em;
    }
  }
}
</style>
