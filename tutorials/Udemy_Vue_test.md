
# Section#2: Basics & Core Concepts - DOM Interface with Vue

`filename: index.html`
``` html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Vue Basics</title>
  <link href="https://fonts.googleapis.com/css2?family=Jost:wght@400;700&display=swap" rel="stylesheet" />
  <link rel="stylesheet" href="styles.css" />
  <script src="https://unpkg.com/vue@next" defer></script>
  <script src="app.js" defer></script>
</head>

<body>
  <header>
    <h1>Vue Events</h1>
  </header>
  <section id="events">
    <h2>Events in Action</h2>
    <button v-on:click="add()">Add</button>
    <button v-on:click="remove()">Remove</button>
    <p v-once>Starting Counter: {{ counter }}</p>
    <p>Result: {{ counter }}</p>
    <input type="text" v-bind:value="name" v-on:input="setName($event, 'Kalisetti')" v-on:keyup.enter="confirmInput"/>
    <button v-on:click="resetInput">Reset Input</button>
    <p>Your Name: {{ confirmedName }}</p>
    <form v-on:submit.prevent="submitForm">
      <input type="text" />
      <button>Sign Up</button>
    </form>
  </section>
</body>

</html>
```

`filename: app.js`
``` js
const app = Vue.createApp({
  data() {
    return {
      counter: 10,
      name: '',
      confirmedName: ''
    };
  },
  methods: {
    resetInput() {
      this.name = '';
    },
    confirmInput() {
      this.confirmedName =this.name;
    },
    submitForm(event){
      // event.preventDefault();
    },
    setName(event, lastName){
      // this.name = event.target.value + ' ' + lastName;
      this.name = event.target.value;
    },
    add(){
      this.counter++;
    },
    remove(){
      this.counter--;
    }
  }
});

app.mount('#events');
```
