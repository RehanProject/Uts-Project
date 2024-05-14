<template>
  <div>
    <header>
      <nav>
        <ul>
          <li><a href="#" @click.prevent="selectMenu('Todos')">Todos</a></li>
          <li><a href="#" @click.prevent="selectMenu('Post')">Post</a></li>
        </ul>
      </nav>
    </header>
    <div v-if="selectedMenu === 'Todos'">
      <!-- Kode untuk fitur todos yang sebelumnya dibuat -->
      <task-list :tasks="tasks" @delete-task="deleteTask" @filter-tasks="filterTasks" />
      <form @submit.prevent="addTask">
        <input type="text" v-model="newTaskName" placeholder="Masukkan Daftar Kegiatan">
        <br>
        <button type="submit">Tambah Kegiatan</button>
        <br>
      </form>
      <button @click="filterTasks">Tampilkan Kegiatan Yang Belum Selesai</button>
    </div>
    <div v-else-if="selectedMenu === 'Post'">
      <!-- Kode untuk fitur postingan -->
      <select v-model="selectedUser">
        <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
      </select>
      <button @click="getPosts">Tampilkan Postingan</button>
      <ul v-if="posts.length > 0">
        <li v-for="post in posts" :key="post.id">
          {{ post.title }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [],
      newTaskName: '',
      selectedMenu: 'Todos',
      selectedUser: '',
      users: [],
      posts: []
    }
  },
  methods: {
    selectMenu(menu) {
      this.selectedMenu = menu;
    },
    addTask() {
      this.tasks.push({ name: this.newTaskName, done: false });
      this.newTaskName = '';
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    filterTasks() {
      this.tasks = this.tasks.filter(task => !task.done);
    },
    getPosts() {
      fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUser}`)
        .then(response => response.json())
        .then(data => {
          this.posts = data;
        });
    }
  }
}
</script>