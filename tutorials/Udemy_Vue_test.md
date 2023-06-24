---
#### Section#2: Basics & Core Concepts - DOM Interface with Vue
---

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

+ Two way binding is simply a shortcut for\
  `v-bind:value="name" v-on:input="setName($event, 'Kalisetti')"`

+ Instead we can use\
  `v-model="name"`

##### Section#2: 28. Methods used for Data Binding: How it works

+ One this is, if we are calling functions directly like as shown below, every time vue re-renders due to some other events
	these functions are called again and again. This could cause performance issue.

```html
<p>Your Name: {{ outputFullName() }}</p>
```

##### Section#2: 29. Introducing Computed Properties

+ We use computed properties like data properties and not like functions, that is why we use names similar to data
	properties eg. fullname

+ Vue basically keeps track of dependencies in the computed methods, and re-evaluate them if any of the dependencies 
	change like in this example this.name

+ We usually use computed properties for displaying, we dont call it from events. From events, we still use functions
	or normal methods.

##### Section#2: 30. Working with watchers

+ Watcher is somewhat similar to computed property, the difference is watch is created directly for a data/computer peroperty
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

+ We dont return anything from watches, instead we use them to execute some code whenever a value changes

+ Ideal in situations like resetting the counter value to zero, timers, HTTP request etc., basically something that should
	execute behind the scenes.

##### Section#2: 31. Methods vs Computer Properties vs Watchers

##### Section#2: 32. v-bind and v-on Shorthands

+ Instead of `v-on` we can use `@`

```js
  v-on:click="add"
  becomes
  @click="add"
```

+ Instead of `v-bind` we can use `:`

```js
  v-bind:value="name"
  becomes
  :value="name"
```

```These are just shortcuts to make code shorter```

##### Section#2: 33. Dynamic Styling with Inline Styles
---

##### Section#2: 34. Adding CSS classes dynamically
---
##### Section#2: 35. Classes and Computed Properties
---
##### Section#2: 36. Dynamic Classes: Array Syntax
---
+ Instead of having one property we can have multiple classes in array style
```js
  :class="{active: boxASelected}"
  becomes
  :class="['demo', {active: boxASelected}]"
```

---
#### Section#3: Rendering Conditional Content & Lists
---

##### Section#3: 41. Rendering Content Conditionally
---

+ <p v-if="goals.length == 0">No goals have been added yet - please start adding some!</p>


##### Section#3: 42. v-if, v-else and v-else-if
---

* v-else needs be the next immediate element after v-if

      <p v-if="goals.length == 0">No goals have been added yet - please start adding some!</p>
      <ul v-else>
        <li>Goal</li>
      </ul>

##### Section#3: 43. Using v-show instead of v-if
---

* v-if removes or inserts elements into DOM. Adding and removing elements from DOM causes performance.
* v-show keeps the elements but makes their css property "display: none" while keeping the element in DOM,
    however, v-show will not have v-else or v-else-if. It works like a standalone condition.

    <p v-show="goals.length == 0">No goals have been added yet - please start adding some!</p>
    <ul v-show="goals.length > 0">
      <li>Goal</li>
    </ul>


##### Section#3: 44. Rendering Lists of Data
---

<li v-for="goal in goals">{{ goal }}</li>


##### Section#3: 45. Diving Deeper Into v-for
---

+ Getting index in the loop
  
```html
<li v-for="(goal, idx) in goals">{{ goal }}</li>
```

+ Loop through object
  
```html
<li v-for="(val, key, idx) in {name: 'Shiv', age: 40}">{{ key }}: {{ val }}</li>
```

+ Loop through range of numbers (1..10)

```html
<li v-for="num in 10">{{ num }}</li>
```

##### Section#3: 46. Removing List Items
---

```html
    <li v-for="(goal,idx) in goals" @click="removeGoal(idx)">{{ goal }}</li>

    removeGoal(idx) {
      this.goals.splice(idx, 1);
    }
```

## Section#3: 47. Lists & Keys
---

+ `@click.stop` can be used to override the parent click
+ One issue here is, when we remove elements, Vue actually moves the content of next element
    into the deleted one making it appear like it has removed, however if there are any values in the input
    elements in following elements, they will become zero. For this purpose we need make each element as unique
    using the vue key, we could use DB primary key here

```html
  <li v-for="(goal,idx) in goals" :key="goal" @click="removeGoal(idx)">
    <p>{{ goal }}</p>
    <input type="text" @click.stop/>
  </li>
```

---
#### Section#4: Course Project: The Monster Slayer Game
---


+ We can put our regular javascript function above the vue app

```js
function getRandomValue(min, max) {
    return Math.floor(Math.random() * (max - min)) + min;
}

const app = Vue.createApp({
    data() {
```


##### Section#5: Vue: Behind the Scenes
---

##### Section#5: 60. An introduction to Vue's Reactivity
---

##### Section#5: 61. Vue Reactivity: A Deep Dive
---

+ By default javascript is not reactive like vue, meaning, when we make changes to data properties it will not trigger
    other dependent codes.

`javascript proxies`
```js
const data = {
  message: 'Hello!'
};

const handler = {
  set(target, key, value) {
    console.log(target);
    console.log(key);
    console.log(value);
  }  
}
```

+ now we can wrap this object with javascript proxie

```js 
const proxy = new Proxy(data, handler);
```

+ now the handler gets called when we change value in data

```js
proxy.message = 'Hello!!!!';
```

+ This prints the following three console logs
```js
{message: 'Hello!'}
message
Hello!!!!
```

+ Now, we want to make the changes as soon as value in message changes, we can re-write our handler like this

```js
const data = {
  message: 'Hello!',
  longMessage: 'Hello World!'
};

const handler = {
  set(target, key, value) {
    if (key === 'message') {
      target.longMessage = value + ' Wordl!'
    }
    target.message = value;
  }  
}

const proxy = new Proxy(data, handler);

proxy.message = 'Hello!!!!';

console.log(proxy.longMessage);
--output
Hello!!!! Wordl!
```

+ So in a nutshell this is what Vue does, it keeps track of data properties, and whenever that property changes it updates
    the part of our app where that property was used.


##### Section#5: 62. One App vs Multiple Apps
---

+ So far we created single app, however we can have as many apps we like and mount them in similar fashion.
+ Each Vue app works standalone, they do not have any links with other apps.


##### Section#5: 63. Understanding Templates
---

+ Basically the html part we used in Vue app are called html template of the app
+ So far, we have html code and we mounted our vue app on it, however we can also have template option when creating the app

```js
const app = Vue.createApp({
  template: `
    <p>{{ favoriteMeal }}</p>
  `,
  data() {
    return {
      favoriteMeal: 'Pizza'
    }
  }
});

app.mount('#app2')
```


##### Section#5: 64. Working with Refs
---

+ Instead of adding event like @input or v-model which is basically executed with every key stroke, we can use ref attribute.
  We can use this ref attribute to access the values on elements.

```js
  <input type="text" ref="userText">

  setText() {
    // this.message = this.currentUserInput;
    this.message = this.$refs.userText.value;  
  }
```


##### Section#5: 65. How Vue Updates the DOM
---

+ Vue actually maintains entire DOM in javascript/memory and whenever changes happen 
  to data properties, it creates another virtual DOM and compares for changes and 
  then updates in real DOM(which was rendered to the screen).

+ In reality it does not re-create the entrire DOM, vue has lot of performance tricks


##### Section#5: 66. Vue App Lifecycle - Theory
---

+ Vue instance lifecycle, we can use these hooks and do cleanup if required and many more

  * createApp({...})
    - beforeCreate()
      called before the app is fully initialized
    - created()
      called thereafter, after this vue knows the data properties and general configuration.
        - Compile template --> beforeMount()
    - beforeMount()
      right before we see something on screen
    - mounted() --> Mounted Vue Instance --> Data Changed --> beforeUpdate() --> updated
                            |
                            --> Instance Unmounted --> beforeUnmount() --> unmounted()
      now we see something on the screen


##### Section#5: 67. Vue App Lifecycle - Practice
---

```js
const app = Vue.createApp({
  data() {
    return {
      currentUserInput: '',
      message: 'Vue is great!',
    };
  },
  methods: {
    saveInput(event) {
      this.currentUserInput = event.target.value;
    },
    setText() {
      console.log('setText()');
      this.message = this.currentUserInput;
    },
  },
  beforeCreate() {
    console.log('beforeCreate()');
  },
  created() {
    console.log('created()');
  },
  beforeMount() {
    console.log('beforeMount()');
  },
  mounted() {
    console.log('mounted()');
  },
  beforeUpdate() {
    console.log('beforeUpdate()');
  },
  updated() {
    console.log('updated()');
  },
  beforeUnmount() {
    console.log('beforeUnmount()');
  },
  unmounted() {
    console.log('unmounted()');
  }
});

app.mount('#app');

setTimeout(function () {
  app.unmount();
}, 3000)
```

