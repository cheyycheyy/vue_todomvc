<template>
  <div class="contant">
    <section class="todoapp">
      <header class="header">
        <h1>todos</h1>
        <!-- 输入框回车添加项目 -->
        <input class="new-todo" placeholder="What needs to be done?" autofocus v-model="newTodo" @keyup.enter="addItem">
      </header>
      <!-- This section should be hidden by default and shown when there are todos -->
      <section class="main">
        <input id="toggle-all" class="toggle-all" type="checkbox">
        <label for="toggle-all">Mark all as complete</label>
        <ul class="todo-list">
          <!-- These are here just to show the structure of the list items -->
          <!-- List items should get the class `editing` when editing and `completed` when marked as completed -->
          <li v-for="(item, index) in projectList" :key="index" 
            :class="{completed: item.isCompleted, editing: item.isEditing}">
            <div class="view">
              <input class="toggle" type="checkbox" v-model="item.isCompleted">
              <!-- 双击编辑todo -->
              <label @dblclick="editItem(item)">{{item.title}}</label>
              <button class="destroy" @click="deleteItem(item)"></button>
            </div>
            <!-- @blur失去焦点时  v-todo-focus自定义指令获取焦点 -->
            <input class="edit" v-model="item.editText" @keyup.enter="editTitle(item)"
             @keyup.esc="cancelEdit(item)"
            v-todo-focus="item.isEditing" @blur="editTitle(item)">
          </li>
        </ul>
      </section>
      <!-- This footer should hidden by default and shown when there are todos -->
      <footer class="footer">
        <!-- This should be `0 items left` by default -->
        <span class="todo-count">
          <strong v-text="projectList.length"></strong> item left</span>
        <!-- Remove this if you don't implement routing -->
        <ul class="filters">
          <li>
            <a class="selected" href="#/">All</a>
          </li>
          <li>
            <a href="#/active">Active</a>
          </li>
          <li>
            <a href="#/completed">Completed</a>
          </li>
        </ul>
        <!-- Hidden if no completed items are left ↓ -->
        <button class="clear-completed" @click="removeCompleted">Clear completed</button>
      </footer>
    </section>

  </div>
</template>




<script>
export default {
  name: 'contant',

  data() {
    return {
      newTodo: '',
      editedTodo: null,
      projectList: [
        {
          title: '123',
          isCompleted: true,
          isEditing: false,
          editText: '123'
        },
        {
          title: '1234',
          isCompleted: false,
          isEditing: false,
          editText: '1234'
        },

      ],

      // 过滤器
      filters: {
        all(projectList) {
          return projectList;
        },
        active(projectList) {
          return projectList.filter(function(todo) {
            return !todo.isCompleted;
          });
        },
        completed(projectList) {
          return projectList.filter(function(todo) {
            return todo.isCompleted;
          });
        }
      },
    }
  },

  // 方法
  methods: {
    deleteItem(item) {
      var index = this.projectList.indexOf(item);
      this.projectList.splice(index, 1);
    },

    addItem() {
      var value = this.newTodo && this.newTodo.trim();
      if (!value) {
        return;
      }
      this.projectList.push({ title: value, isEditing: false, isCompleted: false, editText: value });
      this.newTodo = '';
    },

    // 进入title编辑状态
    editItem(item) {
      item.isEditing = true;
      item.focus();
    },

    // 编辑完成 
    editTitle(item) {
      item.title = item.editText;
      item.isEditing = false;
    },

    cancelEdit(item) {
      item.isEditing = false;
    },

    removeCompleted() {
      this.projectList = this.filters.active(this.projectList);
    }

  },

  // 监视
  watch: {

  },


  // 自定义指令 
  directives: {
    // 获取焦点，el指令所在对象，bingding参数
    'todo-focus'(el, binding) {
      if (binding.value) {
        el.focus();
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
