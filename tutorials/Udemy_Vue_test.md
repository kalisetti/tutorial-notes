
#### Section#2: Basics & Core Concepts - DOM Interface with Vue

`filename: index.html`
```html
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
```js
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

##### Section#2: 27. Data Binding + Event Binding = Two-way Binding

+ Two way binding is simply a shortcut for 
   `v-bind:value="name" v-on:input="setName($event, 'Kalisetti')"`

+ Instead we can use `v-model="name"`

##### Section#2: 28. Methods used for Data Binding: How it works

> One this is, if we are calling functions directly like as shown below, every time vue re-renders due to some other events
	these functions are called again and again. This could cause performance issue.

```html
<p>Your Name: {{ outputFullName() }}</p>
```

##### Section#2: 29. Introducing Computed Properties

> We use computed properties like data properties and not like functions, that is why we use names similar to data
	properties eg. fullname

> Vue basically keeps track of dependencies in the computed methods, and re-evaluate them if any of the dependencies 
	change like in this example this.name

> We usually use computed properties for displaying, we dont call it from events. From events, we still use functions
	or normal methods.

##### Section#2: 30. Working with watchers

* Watcher is somewhat similar to computed property, the difference is watch is created directly for a data/computer peroperty
	variable, when the value of it is changed the watch is executed.

```js
	eg:
		data() {
			name: ''
		},
		watch: {
			name(newValue, oldValue) {
				// Do something here
			}
		}
```

* We dont return anything from watches, instead we use them to execute some code whenever a value changes

* Ideal in situations like resetting the counter value to zero, timers, HTTP request etc., basically something that should
	execute behind the scenes.

##### Section#2: 31. Methods vs Computer Properties vs Watchers

##### Section#2: 32. v-bind and v-on Shorthands

* Instead on v-on we can use @
	```js
    eg: v-on:click="add"
		becomes
		@click="add"
  ```

* `v-bind:value="name"`
	becomes 
	`:value="name"`

* These are just shortcuts to make code shorter

##### Section#2: 33. Dynamic Styling with Inline Styles

##### Section#2: 34. Adding CSS classes dynamically

##### Section#2: 35. Classes and Computed Properties

##### Section#2: 36. Dynamic Classes: Array Syntax

* Instead of having one property we can have multiple classes in array style
  
	`:class="{active: boxASelected}"`
	becomes
	`:class="['demo', {active: boxASelected}]"`

