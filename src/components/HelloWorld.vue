<template>
  <Page class="page">
    <ActionBar class="action-bar" title="Hello world"></ActionBar>

    <TabView height="100%">
      <TabViewItem title="To Do">
        <!-- Positions an input field, a button, and the list of tasks in a grid. -->
        <StackLayout
          orientation="vertical"
          width="100%"
          height="100%"
        >
          <GridLayout
            class="container"
            columns="*, auto"
            rows="auto"
            width="100%"
          >
            <TextField
              row="0"
              col="0"
              v-model="taskTitle"
              hint="Type new task..."
              editable="true"
              @returnPress="addTask"
            /> <!-- Configures the text field and ensures that pressing Return on the keyboard produces the same result as tapping the button. -->
            <Button
              class="add-task-btn"
              row="0"
              col="1"
              text="Add task"
              @tap="addTask"
            />
          </GridLayout>

          <ListView
            v-for="todo in incompleteTodos"
            height="100%"
            @itemTap="onListItemTap"
          > <!-- Make sure to set a height or your list will not show on iOS. -->
            <v-template>
              <Label :text="todo.title" />
            </v-template>
          </ListView>
        </StackLayout> 
      </TabViewItem>

      <TabViewItem title="Completed">
        <ListView
          v-for="todo in completeTodos"
          @itemTap="onCompleteTodoTap"
        >
          <v-template>
            <Label :text="todo.title"></Label>
          </v-template>
        </ListView>
      </TabViewItem>
    </TabView>
  </Page>
</template>

<script>
  export default {
    data () {
      return {
        taskTitle: '',
        todos: [
          { title: 'Buy bread', done: false },
          { title: 'Call to mom', done: true },
          { title: 'Make 2 copies of documents', done: false }
        ],
        text: {
          todoAction: 'What do you want to do with this task?',
          cancel: 'Cancel',
          completeTodo: 'Mark as completed',
          undoneTodo: 'Mark as incompleted',
          deleteTodo: 'Delete forever',
          todoTitleRequired: 'We need todo title.'
        }
      }
    },

    computed: {
      completeTodos () {
        return this.todos
          .filter(todo => todo.done)
      },
      incompleteTodos () {
        return this.todos
          .filter(todo => !todo.done)
      }
    },

    methods: {
      t(key) {
        return this.text[key]
      },

      addTask () {
        let title = this.taskTitle

        if (!title) {
          return alert(this.t('todoTitleRequired'))
        }

        this.todos.push({ title, done: false })
        this.taskTitle = ''
      },

      onListItemTap ({ index, item }) {
        return action(
          this.t('todoAction'),
          this.t('cancel'),
          [this.t('completeTodo'), this.t('deleteTodo')]
        )
          .then(result => {
            switch (result) {
              case this.t('completeTodo'):
                item.done = true
                break

              case this.t('deleteTodo'):
                // Removes the tapped todo.
                this.todos.splice(index, 1)
                break
            }
          })
      },

      onCompleteTodoTap ({ item, index }) {
        return action(
          this.t('todoAction'),
          this.t('cancel'),
          [this.t('undoneTodo'), this.t('deleteTodo')]
        )
          .then(res => {
            switch (res) {
              case this.t('undoneTodo'):
                item.done = false
                break

              case this.t('deleteTodo'):
                this.todos.splice(index, 1)
                break
            }
          })
      }
    }
  }
</script>

<style scoped>
  .logo-container {
    /* background: gold; */
  }

  .container {
    padding-left: 15;
    padding-right: 15;
  }

  .logo {
    margin-top: -30;
  }

  .hello-world {
    margin: 20;
  }
</style>