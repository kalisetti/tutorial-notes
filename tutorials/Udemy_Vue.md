--
-- 2023/04/30
-- Udemy - VUE
--
--

---
# Section#2: Basics & Core Concepts - DOM Interface with Vue
---

`-- index.html`
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

`-- app.js`
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

---
## Section#2: 27. Data Binding + Event Binding = Two-way Binding
---

+ Two way binding is simply a shortcut for 
  + `v-bind:value="name" v-on:input="setName($event, 'Kalisetti')"`

+ instead we can use `v-model="name"`

---
## Section#2: 28. Methods used for Data Binding: How it works
---

> One this is, if we are calling functions directly like as shown below, every time vue re-renders due to some other events
	these functions are called again and again. This could cause performance issue.

``` html
<p>Your Name: {{ outputFullName() }}</p>
```

---
## Section#2: 29. Introducing Computed Properties
---

> We use computed properties like data properties and not like functions, that is why we use names similar to data
	properties eg. fullname

> Vue basically keeps track of dependencies in the computed methods, and re-evaluate them if any of the dependencies 
	change like in this example this.name

> We usually use computed properties for displaying, we dont call it from events. From events, we still use functions
	or normal methods.

---
## Section#2: 30. Working with watchers
---

* Watcher is somewhat similar to computed property, the difference is watch is created directly for a data/computer peroperty
	variable, when the value of it is changed the watch is executed.

``` js
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

---
## Section#2: 31. Methods vs Computer Properties vs Watchers
---

---
## Section#2: 32. v-bind and v-on Shorthands
---

* Instead on v-on we can use @
	eg: v-on:click="add"
		becomes
		@click="add"

* v-bind:value="name"
	becomes 
	:value="name"

* These are just shortcuts to make code shorter

---
## Section#2: 33. Dynamic Styling with Inline Styles
---

---
## Section#2: 34. Adding CSS classes dynamically
---

---
## Section#2: 35. Classes and Computed Properties
---

---
## Section#2: 36. Dynamic Classes: Array Syntax
---

* Instead of having one property we can have multiple classes in array style

	:class="{active: boxASelected}"
	becomes
	:class="['demo', {active: boxASelected}]"


---
# Section#3: Rendering Conditional Content & Lists
---

---
## Section#3: 41. Rendering Content Conditionally
---

<p v-if="goals.length == 0">No goals have been added yet - please start adding some!</p>


---
## Section#3: 42. v-if, v-else and v-else-if
---

* v-else needs be the next immediate element after v-if

      <p v-if="goals.length == 0">No goals have been added yet - please start adding some!</p>
      <ul v-else>
        <li>Goal</li>
      </ul>

---
## Section#3: 43. Using v-show instead of v-if
---

* v-if removes or inserts elements into DOM. Adding and removing elements from DOM causes performance.
* v-show keeps the elements but makes their css property "display: none" while keeping the element in DOM,
    however, v-show will not have v-else or v-else-if. It works like a standalone condition.

    <p v-show="goals.length == 0">No goals have been added yet - please start adding some!</p>
    <ul v-show="goals.length > 0">
      <li>Goal</li>
    </ul>

---
## Section#3: 44. Rendering Lists of Data
---

<li v-for="goal in goals">{{ goal }}</li>

---
## Section#3: 45. Diving Deeper Into v-for
---

-- Getting index in the loop
<li v-for="(goal, idx) in goals">{{ goal }}</li>

-- Loop through object
<li v-for="(val, key, idx) in {name: 'Shiv', age: 40}">{{ key }}: {{ val }}</li>

-- Loop through range of numbers (1..10)
<li v-for="num in 10">{{ num }}</li>

---
## Section#3: 46. Removing List Items
---

    <li v-for="(goal,idx) in goals" @click="removeGoal(idx)">{{ goal }}</li>

    removeGoal(idx) {
      this.goals.splice(idx, 1);
    }

---
## Section#3: 47. Lists & Keys
---
* @click.stop can be used to override the parent click
* One issue here is, when we remove elements, Vue actually moves the content of next element
    into the deleted one making it appear like it has removed, however if there are any values in the input
    elements in following elements, they will become zero. For this purpose we need make each element as unique
    using the vue key, we could use DB primary key here

  <li v-for="(goal,idx) in goals" :key="goal" @click="removeGoal(idx)">
    <p>{{ goal }}</p>
    <input type="text" @click.stop/>
  </li>


---
# Section#4: Course Project: The Monster Slayer Game
---


* We can put our regular javascript function above the vue app

function getRandomValue(min, max) {
    return Math.floor(Math.random() * (max - min)) + min;
}

const app = Vue.createApp({
    data() {



---
# Section#5: Vue: Behind the Scenes
---



---
## Section#5: 60. An introduction to Vue's Reactivity
---

---
## Section#5: 61. Vue Reactivity: A Deep Dive
---

* by default javascript is not reactive like vue, meaning, when we make changes to data properties it will not trigger
    other dependent codes.

`-- javascript proxies`
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

* now we can wrap this object with javascript proxie
const proxy = new Proxy(data, handler);

* now the handler gets called when we change value in data
proxy.message = 'Hello!!!!';

-- this prints the following three console logs
{message: 'Hello!'}
message
Hello!!!!

* Now, we want to make the changes as soon as value in message changes, we can re-write our handler like this

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

* So in a nutshell this is what Vue does, it keeps track of data properties, and whenever that property changes it updates
    the part of our app where that property was used.

---
## Section#5: 62. One App vs Multiple Apps
---

* So far we created single app, however we can have as many apps we like and mount them in similar fashion.
* Each Vue app works standalone, they do not have any links with other apps.

---
## Section#5: 63. Understanding Templates
---

* Basically the html part we used in Vue app are called html template of the app
* So far, we have html code and we mounted our vue app on it, however we can also have template option when creating the app

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

---
## Section#5: 64. Working with Refs
---

* Instead of adding event like @input or v-model which is basically executed with every key stroke, we can use ref attribute.
  We can use this ref attribute to access the values on elements.

  <input type="text" ref="userText">

  setText() {
    // this.message = this.currentUserInput;
    this.message = this.$refs.userText.value;  
  }

---
## Section#5: 65. How Vue Updates the DOM
---

* Vue actually maintains entire DOM in javascript/memory and whenever changes happen 
  to data properties, it creates another virtual DOM and compares for changes and 
  then updates in real DOM(which was rendered to the screen).

* In reality it does not re-create the entrire DOM, vue has lot of performance tricks

---
## Section#5: 66. Vue App Lifecycle - Theory
---

* Vue instance lifecycle, we can use these hooks and do cleanup if required and many more

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

---
## Section#5: 67. Vue App Lifecycle - Practice
---

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


---
# Section#6: Introducing Components
---


* as noticed, button in v-for loop is getting executed for all the elements listed via 
  loop, here we can use components


>>>>> index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vue Basics</title>
    <link
      href="https://fonts.googleapis.com/css2?family=Jost:wght@400;700&display=swap"
      rel="stylesheet"
    />
    <link rel="stylesheet" href="styles.css" />
    <script src="https://unpkg.com/vue@next" defer></script>
    <script src="app.js" defer></script>
  </head>
  <body>
    <header>
      <h1>FriendList</h1>
    </header>
    <section id="app">
      <ul>
        <friend-contact></friend-contact>
        <friend-contact></friend-contact>
      </ul>
    </section>
  </body>
</html>

>>>>> app.js
const app = Vue.createApp({
    data() {
        return {
            detailsAreVisible: false,
            friends: [
                {id: 'manuel', name: 'Manuel Lorenz', phone: '01234 5678 991', email: 'manuel@localhost.com'},
                {id: 'julie', name: 'Julie Jones', phone: '09876 543 221', email: 'julie@localhost.com'}
            ]
        }
    },
    // methods: {
    //     toggleDetails() {
    //         this.detailsAreVisible = !this.detailsAreVisible;
    //     }
    // }
});

app.component('friend-contact', {
    template: `
    <li>
    <h2>{{ friend.name }}</h2>
    <button @click="toggleDetails">
      {{ detailsAreVisible ? 'Hide' : 'Show' }} Details
    </button>
    <ul v-if="detailsAreVisible">
      <li><strong>Phone:</strong> {{ friend.phone }}</li>
      <li><strong>Email:</strong> {{ friend.email }}</li>
    </ul>
    </li>
    `,
    data() {
        return {
            detailsAreVisible: false,
            friend: {id: 'manuel', name: 'Manuel Lorenz', phone: '01234 5678 991', email: 'manuel@localhost.com'}
        }
    },
    methods: {
        toggleDetails() {
            this.detailsAreVisible = !this.detailsAreVisible;
        }
    }
});

app.mount('#app');

>>>>> styles.css
* {
  box-sizing: border-box;
}

html {
  font-family: 'Jost', sans-serif;
}

body {
  margin: 0;
}

header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 3rem auto;
  border-radius: 10px;
  padding: 1rem;
  background-color: #58004d;
  color: white;
  text-align: center;
  width: 90%;
  max-width: 40rem;
}

#app ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

#app li {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 1rem auto;
  border-radius: 10px;
  padding: 1rem;
  text-align: center;
  width: 90%;
  max-width: 40rem;
}

#app h2 {
  font-size: 2rem;
  border-bottom: 4px solid #ccc;
  color: #58004d;
  margin: 0 0 1rem 0;
}

#app button {
  font: inherit;
  cursor: pointer;
  border: 1px solid #ff0077;
  background-color: #ff0077;
  color: white;
  padding: 0.05rem 1rem;
  box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.26);
}

#app button:hover,
#app button:active {
  background-color: #ec3169;
  border-color: #ec3169;
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.26);
}



---
# Section#6: 71. Introducing Components
---

* Components are useful when you reuse certain parts of the html code and enclose 
  certain functionality in that html block specific to that block.
* Component is like a custom html element
* Components are like mini apps that are linked to the main app

* Component example
app.component('friend-contact');
app.mount();

  - first argument: is user defined html tag(preferrable with hiphen) like 
    'user-contact' etc.,
  - second argument: config object, which is similar to our app object


---
# Section#7: Moving to a Better Development Setup & Workflow with the Vue CLI
---

---
## Section#7: 78. Fixing npm run serve (Vue CLI)
---

* Fixing npm run serve (Vue CLI)
In the next lecture, we will use a third-party tool (Vue CLI)  to create a new project. 
This tool, under the hood, uses NodeJS - a software which you need to download as part 
of the next lecture.

* You will learn about the details in the next lectures but in case you''re getting any 
  errors when trying to run Vue projects (via npm run serve, after creating them via 
  the Vue CLI), try the following adjustment:

  Replace the scripts entry in your project''s package.json before running npm run serve:

  Windows:

  "scripts": {  
    "serve": "set NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service serve",  
    "build": "set NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service build",  
    "lint": "set NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service lint"
  },

  Mac and Linux:

  "scripts": {  
    "serve": "export NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service serve",  
    "build": "export NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service build",  
    "lint": "export NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service lint"
  },

---
## Section#7: 79. Installing & Using the Vue CLI
---

* install vue cli
  
  > npm install -g @vue/cli

* create project

  > vue create vue-first-app

  🎉  Successfully created project vue-first-app.
  👉  Get started with the following commands:

   $ cd vue-first-app
   $ npm run serve

---
## Section#7: 80. Inspecting the Created Project
---

---
## Section#7: 81. Inspecting the Vue Code & ".vue" Files
---

---
## Section#7: 82. Adding the "Vetur" Extension to VS Code
---

---
## Section#7: 83. More on ".vue" Files
---


---
## Section#7: 85. Creating a Basic Vue App
---

>>>>> create App.vue // can be named anything and put our code

  <template>
  <section>
      <h2>My Friends</h2>
      <ul>
          <li v-for="friend in friends" :key="friend.id" >{{ friend.name }}</li>
      </ul>
  </section>
  </template>

  <script>
  //const app = {           // We better not use const here, because it becomes available only within the file
    export default {        // We make our object a default export, so that it can be used in main.js
      data() {
          return {
              friends: [
                  {id: 'manuel', name: 'Manuel Lorenz', phone: '0123 45678 90', email: 'manuel@localhost.com'},
                  {id: 'julie', name: 'Julie Jones', phone: '0982 16546 00', email: 'julie@localhost.com'},
              ]
          }
      }
  }
  </script>

- main.js
  import { createApp } from 'vue';  // import from the package called vue
  import App from ./App.vue;  // here we can use any name instead of App, basically it imports everything from App.vue into this name

  createApp(App).mount('#app');

---
## Section#7: 86. Adding a Component
---

* Components are simply like mini vue apps with their own data properties and methods, 
  which are then linked to an app
* It is a convention to have separate folder for components
* Create src/components

- src/components/FriendContact.vue
<template>
    <li>
        <h2>{{ friend.name }}</h2>
        <button @click="toggleDetails">Show Details</button>
        <ul v-if="detailsAreVisible">
            <li><strong>Phone:</strong> {{ friend.phone }}</li>
            <li><strong>Email:</strong> {{ friend.email }}</li>
        </ul>
    </li>
</template>

<script>
export default {
    data() {
        return {
            detailsAreVisible: false,
            friend: {id: 'manuel', name: 'Manuel Lorenz', phone: '0123 45678 90', email: 'manuel@localhost.com'}
        }
    },
    methods: {
        toggleDetails() {
            this.detailsAreVisible = !this.detailsAreVisible;
        }
    }
}
</script>

- src/App.vue
<template>
<section>
    <h2>My Friends</h2>
    <ul>
        <friend-contact></friend-contact>
        <friend-contact></friend-contact>
    </ul>
</section>
</template>

<script>
export default {
    data() {
        return {
            friends: [
                {id: 'manuel', name: 'Manuel Lorenz', phone: '0123 45678 90', email: 'manuel@localhost.com'},
                {id: 'julie', name: 'Julie Jones', phone: '0982 16546 00', email: 'julie@localhost.com'},
            ]
        }
    }
}
</script>

- src/main.js
import { createApp } from 'vue';
import App from './App.vue';
import FriendContact from './components/FriendContact.vue';

const app = createApp(App);

app.component('friend-contact', FriendContact);

app.mount('#app');

---
## Section#7: 87. Adding Styling
---

- Add <style></style> under App.vue, for now copy the styles from previous component example


---
## Section#7: 89. An Alternative Setup - using "npm init" & Volar
---

* An Alternative Setup - using "npm init" & Volar
  
  The Vue ecosystem keeps on advancing and developing and therefore, there now are official 
  alternatives to using the Vue CLI & Vetur.

  You CAN still use those tools (and in this course, these are the tools being used 
  - so to follow along smoothly, you might want to use them as well).

* But, alternatively and 100% optionally to using the Vue CLI and Vetur, you can also use 
  two different tools / approaches:

  1. Use "npm init vue" instead of installing and using the Vue CLI
  2. Use the Volar extension instead of Vetur

* You don''t have to use either of the two, but you may want to experiment with them. 
  The Vue code you write, is of course 100% the same as shown in this course - no matter 
  which setup you''re using.

* "npm init vue" uses an official package to help you initialize Vue projects. 
  You get a command line wizard that walks you through project creation, 
  comparable to what you get with the Vue CLI (though with slightly different 
  choices and options). For a basic Vue project, you can select "No" for all options.

'
````````
EXAMPLE
````````
'
> npm init vue
npx: installed 1 in 2.576s

Vue.js - The Progressive JavaScript Framework

✔ Project name: … vue-project
✔ Add TypeScript? … No / Yes
✔ Add JSX Support? … No / Yes
✔ Add Vue Router for Single Page Application development? … No / Yes
✔ Add Pinia for state management? … No / Yes
✔ Add Vitest for Unit Testing? … No / Yes
✔ Add an End-to-End Testing Solution? › No
✔ Add ESLint for code quality? … No / Yes

Scaffolding project in /Users/phoenix/Dropbox/Mac/Documents/vue-projects/vue-project1/vue-project...

Done. Now run:

  cd vue-project
  npm install
  npm run dev

> cd vue-project
> npm install
> npm run dev


---
# Section#8: Component Communication
---

---
## Section#8: 92. Introducing "Props" (Parent => Child Communication)
---

* Earlier example was displaying data only from component, how do we pass data from main 
  to component?
* Props shortcut for properties
* Add new key called props in component file object
* props will act same like data properties of the app/component, we could simply call 
  them like this.phoneNumber etc.,
* We can pass the properties from master to components using custom property name, these 
  names should be in kebab case in master, template file, and should be in camelcase 
  in component object


-- FriendContact.vue
<template>
  <li>
    <h2>{{ name }}</h2>
    <button @click="toggleDetails">{{ detailsAreVisible ? 'Hide' : 'Show' }} Details</button>
    <ul v-if="detailsAreVisible">
      <li>
        <strong>Phone:</strong>
        {{ phoneNumber }}
      </li>
      <li>
        <strong>Email:</strong>
        {{ emailAddress }}
      </li>
    </ul>
  </li>
</template>

<script>
export default {
  props: ['name', 'phoneNumber', 'emailAddress'],
  data() {
    return {
      detailsAreVisible: false,
      friend: {
        id: "manuel",
        name: "Manuel Lorenz",
        phone: "0123 45678 90",
        email: "manuel@localhost.com",
      },
    };
  },
  methods: {
    toggleDetails() {
      this.detailsAreVisible = !this.detailsAreVisible;
    }
  }
};
</script>

-- App.vue
<template>
  <section>
    <header>
      <h1>My Friends</h1>
    </header>
    <ul>
      <friend-contact name="Shiv" phone-number="17115380" email-address="siva@test.com"></friend-contact>
    </ul>
  </section>
</template>

<script>
export default {
  data() {
    return {
      friends: [
        {
          id: "manuel",
          name: "Manuel Lorenz",
          phone: "0123 45678 90",
          email: "manuel@localhost.com",
        },
        {
          id: "julie",
          name: "Julie Jones",
          phone: "0987 654421 21",
          email: "julie@localhost.com",
        },
      ],
    };
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
html {
  font-family: "Jost", sans-serif;
}
body {
  margin: 0;
}
header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 3rem auto;
  border-radius: 10px;
  padding: 1rem;
  background-color: #58004d;
  color: white;
  text-align: center;
  width: 90%;
  max-width: 40rem;
}
#app ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
#app li {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 1rem auto;
  border-radius: 10px;
  padding: 1rem;
  text-align: center;
  width: 90%;
  max-width: 40rem;
}
#app h2 {
  font-size: 2rem;
  border-bottom: 4px solid #ccc;
  color: #58004d;
  margin: 0 0 1rem 0;
}
#app button {
  font: inherit;
  cursor: pointer;
  border: 1px solid #ff0077;
  background-color: #ff0077;
  color: white;
  padding: 0.05rem 1rem;
  box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.26);
}
#app button:hover,
#app button:active {
  background-color: #ec3169;
  border-color: #ec3169;
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.26);
}
</style>

---
## Section#8: 93. Prop Behavior & Changing Props
---

* mutation, what is it?
  - Changing values of properties of props in the component code gives error
      error Unexpected mutation of "isFavorite" prop
  - Vue uses concept called uni-directional dataflow. Meaning that the data passed 
    from master to component should only be changed in master, not in component.

  - If we still have to change, there are two ways
    1) We will let the master app know that we are going to change the value of that property
    2) Save the value passed from master app into a local variable of component and then use it

* Here the second way is implemented

-- FriendContact.vue
<template>
  <li>
    <h2>{{ name }} {{ friendIsFavorite === '1' ? '(Favorite)' : ''}}</h2>
    <button @click="toggleFavorite">Toggle Favorite</button>
    <button @click="toggleDetails">{{ detailsAreVisible ? 'Hide' : 'Show' }} Details</button>
    <ul v-if="detailsAreVisible">
      <li>
        <strong>Phone:</strong>
        {{ phoneNumber }}
      </li>
      <li>
        <strong>Email:</strong>
        {{ emailAddress }}
      </li>
    </ul>
  </li>
</template>

<script>
export default {
  props: ['name', 'phoneNumber', 'emailAddress', 'isFavorite'],
  data() {
    return {
      detailsAreVisible: false,
      friend: {
        id: "manuel",
        name: "Manuel Lorenz",
        phone: "0123 45678 90",
        email: "manuel@localhost.com",
      },
      friendIsFavorite: this.isFavorite   // Storing the prop from master into component local
    };
  },
  methods: {
    toggleDetails() {
      this.detailsAreVisible = !this.detailsAreVisible;
    },
    toggleFavorite() {
      if (this.friendIsFavorite === '1') {
        this.friendIsFavorite = '0';
      } else {
        this.friendIsFavorite = '1';
      }
    }
  }
};
</script>

-- App.vue
<template>
  <section>
    <header>
      <h1>My Friends</h1>
    </header>
    <ul>
      <friend-contact name="Shiv" phone-number="17115380" email-address="siva@test.com" is-favorite="0"></friend-contact>
      <friend-contact name="Addy" phone-number="17256594" email-address="addy@test.com" is-favorite="1"></friend-contact>
    </ul>
  </section>
</template>

<script>
export default {
  data() {
    return {
      friends: [
        {
          id: "manuel",
          name: "Manuel Lorenz",
          phone: "0123 45678 90",
          email: "manuel@localhost.com",
        },
        {
          id: "julie",
          name: "Julie Jones",
          phone: "0987 654421 21",
          email: "julie@localhost.com",
        },
      ],
    };
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
</style>

-- main.js
import { createApp } from 'vue';

import App from './App.vue';
import FriendContact from './components/FriendContact.vue';

const app = createApp(App);

app.component('friend-contact', FriendContact);

app.mount('#app');

-- index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width,initial-scale=1.0" />
    <link rel="icon" href="<%= BASE_URL %>favicon.ico" />
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <noscript>
      <strong
        >We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work
        properly without JavaScript enabled. Please enable it to
        continue.</strong
      >
    </noscript>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>

---
## Section#8: 94. Validating Props
---

* If we want to provide more information about props, we can add datatypes the component props expect etc.,

  // props: ['name', 'phoneNumber', 'emailAddress', 'isFavorite'],
  props: {
    name: String,
    phoneNumber: String,
    emailAddress: String,
    isFavorite: String
  }

or

  props: {
    name: {type: String, required: true},
    phoneNumber: {type: String, required: true},
    emailAddress: {type: String, required: true},
    isFavorite: {
      type: String, 
      required: false, 
      default: '0',
      validator: function(value){     // validator is to make sure we get only 1 or 0 values, otherwise it returns false, we can see this in console
        return value === '1' || value === '0';
      }
    }
  },

---
## Section#8: 95. Supported Prop Values
---

* Supported Prop Values
  In general, you can learn all about prop validation in the official docs: 

  https://v3.vuejs.org/guide/component-props.html

* Specifically, the following value types (type property) are supported:

  String
  Number
  Boolean
  Array
  Object
  Date
  Function
  Symbol

* But type can also be any constructor function (built-in ones like Date or custom ones).

---
## Section#8: 96. Working with Dynamic Prop Values
---

* so far we have been passing values to props as strings, however we can pass 
  dynamic values like boolean etc., by binding the properties

-- FriendContact.vue
// isFavorite: {
    //   type: String, 
    //   required: false, 
    //   default: '0',
    //   validator: function(value){
    //     return value === '1' || value === '0';
    //   }
    // }
    isFavorite: {
      type: Boolean, 
      required: false, 
      default: false,
      // validator: function(value){
      //   return value === '1' || value === '0';
      // }
    }

-- App.vue
<template>
  <section>
    <header>
      <h1>My Friends</h1>
    </header>
    <ul>
      <friend-contact 
        v-for="friend in friends"
        :key="friend.id"
        :name="friend.name" 
        :phone-number="friend.phone" 
        :email-address="friend.email" 
        :is-favorite="true">
      </friend-contact>
    </ul>
  </section>
</template>

---
## Section#8: 97. Emitting Custom Events (Child => Parent Communication)
---

* Communication from the component to the parent
* If a component wants the parent know that something is changed, then the component 
  should emit the message which the parent can listen to and make necessary changes 
  in parent object.
* We can emit own custom events from components

-- FriendContact.vue
    toggleFavorite() {
      // if (this.friendIsFavorite === '1') {
      //   this.friendIsFavorite = '0';
      // } else {
      //   this.friendIsFavorite = '1';
      // }

      // this.friendIsFavorite = !this.friendIsFavorite;
      
      this.$emit('toggle-favorite');
    }


* We can pass as many arguments as we want in emit

-- FriendContact.vue
<template>
  <li>
    <h2>{{ name }} {{ isFavorite ? '(Favorite)' : ''}}</h2>
    <button @click="toggleFavorite">Toggle Favorite</button>
    <button @click="toggleDetails">{{ detailsAreVisible ? 'Hide' : 'Show' }} Details</button>
    <ul v-if="detailsAreVisible">
      <li>
        <strong>Phone:</strong>
        {{ phoneNumber }}
      </li>
      <li>
        <strong>Email:</strong>
        {{ emailAddress }}
      </li>
    </ul>
  </li>
</template>

<script>
export default {
  props: {
    id: {type: String, required: true},
    name: {type: String, required: true},
    phoneNumber: {type: String, required: true},
    emailAddress: {type: String, required: true},
    isFavorite: {
      type: Boolean, 
      required: false, 
      default: false,
    }
  },
  data() {
    return {
      detailsAreVisible: false,
    };
  },
  methods: {
    toggleDetails() {
      this.detailsAreVisible = !this.detailsAreVisible;
    },
    toggleFavorite() {
      this.$emit('toggle-favorite', this.id);   // Custom event
    }
  }
};
</script>

-- App.vue
<template>
  <section>
    <header>
      <h1>My Friends</h1>
    </header>
    <ul>
      <friend-contact 
        v-for="friend in friends"
        :key="friend.id"
        :id="friend.id"
        :name=friend.name
        :phone-number="friend.phone" 
        :email-address="friend.email" 
        :is-favorite=friend.isFavorite
        @toggle-favorite="toggleFavoriteStatus">
      </friend-contact>
    </ul>
  </section>
</template>

<script>
export default {
  data() {
    return {
      friends: [
        {
          id: "manuel",
          name: "Manuel Lorenz",
          phone: "0123 45678 90",
          email: "manuel@localhost.com",
          isFavorite: false,
        },
        {
          id: "julie",
          name: "Julie Jones",
          phone: "0987 654421 21",
          email: "julie@localhost.com",
          isFavorite: true
        },
      ],
    };
  },
  methods: {
    toggleFavoriteStatus(friendId) {
      const identifiedFriend = this.friends.find(friend => friend.id === friendId);
      identifiedFriend.isFavorite = !identifiedFriend.isFavorite;
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
}
</style>

---
## Section#8: 98. Defining & Validating Custom Events
---

* Just like props, we can define our custom events to let the Vue know of events that our 
  components will emit, this is a recommended practice. This is basically done for 
  documentation, so that the team will know what custom events are emitted from 
  the components.

-- FriendContact.vue
<script>
export default {
  props: {
    id: {type: String, required: true},
    name: {type: String, required: true},
    phoneNumber: {type: String, required: true},
    emailAddress: {type: String, required: true},
    isFavorite: {
      type: Boolean, 
      required: false, 
      default: false,
    }
  },
  // emits: ['toggle-favorite'],
  emits: {
    'toggle-favorite': function(id) {       // event validation function
      if (id) {
        return true;
      } else {
        console.warn('Id is missing!');
        return false;
      }
    }
  },
  data() {
    return {
      detailsAreVisible: false,
    };
  },
  methods: {
    toggleDetails() {
      this.detailsAreVisible = !this.detailsAreVisible;
    },
    toggleFavorite() {
      this.$emit('toggle-favorite', this.id);
    }
  }
};
</script>


---
## Section#8: 99. Prop / Event Fallthrough & Binding All Props
---

* Prop / Event Fallthrough & Binding All Props
  
  There are two advanced concepts you also should have heard about:

  1. Prop Fallthrough
  2. Binding All Props on a Component

* Prop Fallthrough

  You can set props (and listen to events) on a component which you havent registered 
  inside of that component.

For example:-

- BaseButton.vue

<template>  
  <button>
    <slot></slot>
  </button>
</template>
 
<script>export default {}</script>

  This button component (which might exist to set up a button with some default styling) 
  has no special props that would be registered.

  Yet, you can use it like this:

<base-button type="submit" @click="doSomething">Click me</base-button>

  Neither the "type" prop nor a custom click event are defined or used in the BaseButton 
  component.

  Yet, this code would work.

  Because Vue has built-in support for prop (and event) "fallthrough".

  Props and events added on a custom component tag automatically fall through to the root 
  component in the template of that component. In the above example, "type" and "@click" 
  get added to the <button> in the "BaseButton" component.

  You can get access to these fallthrough props on a built-in "$attrs" 
  property (e.g. this.$attrs).

  This can be handy to build "utility" or pure presentational components where you 
  don''t want to define all props and events individually.

  You''ll see this in action the component course project ("Learning Resources App") later.

  You can learn more about this behavior here: https://v3.vuejs.org/guide/component-attrs.html


* Binding all Props

  There is another built-in feature/ behavior related to props.

If you have this component:

- UserData.vue

<template>
  <h2>{{ firstname }} {{ lastname }}</h2>
</template>
 
<script>
  export default {
    props: ['firstname', 'lastname']
  }
</script>

You could use it like this:

<template>
  <user-data :firstname="person.firstname" :lastname="person.lastname"></user-data>
</template>
 
<script>
  export default {
    data() {
      return {
        person: { firstname: 'Max', lastname: 'Schwarz' }
      };
    }
  }
</script>

But if you have an object which holds the props you want to set as properties, 
you can also shorten the code a bit:

<template>
  <user-data v-bind="person"></user-data>
</template>
 
<script>
  export default {
    data() {
      return {
        person: { firstname: 'Max', lastname: 'Schwarz' }
      };
    }
  }
</script>

With v-bind="person" you pass all key-value pairs inside of person as props to the component. 
That of course requires person to be a JavaScript object.

This is purely optional but it''s a little convenience feature that could be helpful.

Resources for this lecture


---
## Section#8: 100. Adding Components & Connecting Them
---

---
## Section#8: 101. Adding More Component Communication
---

---
## Section#8: 102. A Potential Problem
---

* Passing the emit thourgh several components

-- main.js
import { createApp } from 'vue';

import App from './App.vue';
import ActiveElement from './components/ActiveElement.vue';
import KnowledgeBase from './components/KnowledgeBase.vue';
import KnowledgeElement from './components/KnowledgeElement.vue';
import KnowledgeGrid from './components/KnowledgeGrid.vue';

const app = createApp(App);

app.component('active-element', ActiveElement);
app.component('knowledge-base', KnowledgeBase);
app.component('knowledge-element', KnowledgeElement);
app.component('knowledge-grid', KnowledgeGrid);

app.mount('#app');


-- App.vue
<template>
  <div>
    <active-element
      :topic-title="activeTopic && activeTopic.title"
      :text="activeTopic && activeTopic.fullText"
    ></active-element>
    <knowledge-base :topics="topics" @select-topic="activateTopic"></knowledge-base>
  </div>
</template>

<script>
export default {
  data() {
    return {
      topics: [
        {
          id: 'basics',
          title: 'The Basics',
          description: 'Core Vue basics you have to know',
          fullText:
            'Vue is a great framework and it has a couple of key concepts: Data binding, events, components and reactivity - that should tell you something!',
        },
        {
          id: 'components',
          title: 'Components',
          description:
            'Components are a core concept for building Vue UIs and apps',
          fullText:
            'With components, you can split logic (and markup) into separate building blocks and then combine those building blocks (and re-use them) to build powerful user interfaces.',
        },
      ],
      activeTopic: null,
    };
  },
  methods: {
    activateTopic(topicId) {
      this.activeTopic = this.topics.find((topic) => topic.id === topicId);
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
section {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 2rem auto;
  max-width: 40rem;
  padding: 1rem;
  border-radius: 12px;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
}

li {
  border-radius: 12px;
  border: 1px solid #ccc;
  padding: 1rem;
  width: 15rem;
  margin: 0 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

h2 {
  margin: 0.75rem 0;
  text-align: center;
}

button {
  font: inherit;
  border: 1px solid #c70053;
  background-color: #c70053;
  color: white;
  padding: 0.75rem 2rem;
  border-radius: 30px;
  cursor: pointer;
}

button:hover,
button:active {
  background-color: #e24d8b;
  border-color: #e24d8b;
}
</style>

-- ./components/ActiveElement.vue
<template>
  <section>
    <h2>{{ topicTitle }}</h2>
    <p>{{ text }}</p>
  </section>
</template>

<script>
export default {
  props: ['topicTitle', 'text'],
};
</script>

-- ./components/KnowledgeBase
<template>
  <section>
    <h2>Select a Topic</h2>
    <knowledge-grid :topics="topics" @select-topic="$emit('select-topic', $event)"></knowledge-grid>
  </section>
</template>

<script>
export default {
  props: ['topics'],
  emits: ['select-topic'],
};
</script>

-- ./components/KnowledgeGrid.vue
<template>
  <ul>
    <knowledge-element
      v-for="topic in topics"
      :key="topic.id"
      :id="topic.id"
      :topic-name="topic.title"
      :description="topic.description"
      @select-topic="$emit('select-topic', $event)"
    ></knowledge-element>
  </ul>
</template>

<script>
export default {
  props: ['topics'],
  emits: ['select-topic']
};
</script>

-- ./components/KnowledgeElement.vue
<template>
  <li>
    <h3>{{ topicName }}</h3>
    <p>{{ description }}</p>
    <button @click="$emit('select-topic', id)">Learn More</button>
  </li>
</template>

<script>
export default {
  props: ['id', 'topicName', 'description'],
  emits: ['select-topic'],
};
</script>


---
## Section#8: 103. Provide + Inject to the rescue
---

* Above approach passes the emit thourgh several components, which works fine but is bit cumbersome,
    moreover the components in the middle dont make use of the emits except for passing through 
    to the next components. This is creating bit confusion when it comes to readability of the code.
* Instead of passing the props & emit through each component, vue has better option 
* We can use provides for master app/component to provide the data to children components
* Children component can receive the data through inject
* for emits, we can think of a better way instead of emitting through all components


---
## Section#8: 103. Provide + Inject to the rescue
---

-- App.vue
provide() {
  return {
    'topics': this.topics
  }
},

-- KnowledgeGrid.vue
<script>
export default {
  inject: ['topics'],
  emits: ['select-topic']
};
</script>

---
## Section#8: 104. Provide + Inject for Functions / Methods
---

* Currently, we are emitting the selected topic id all the way to main app, 
  and then execute the method to show the selected topic
* Instead of this, we could pass our topicSelected function from vue app 
  to components, which can execute on their own

-- App.vue
  provide() {
    return {
      topics: this.topics,
      selectTopic: this.activateTopic
    }
  },
  methods: {
    activateTopic(topicId) {
      this.activeTopic = this.topics.find((topic) => topic.id === topicId);
    },
  },

-- KnowledgeElement.vue
<template>
  <li>
    <h3>{{ topicName }}</h3>
    <p>{{ description }}</p>
    <button @click="selectTopic(id)">Learn More</button>
  </li>
</template>

<script>
export default {
  inject: ['selectTopic'],
  props: ['id', 'topicName', 'description'],
};
</script>

---
## Section#8: 104. Provide + Inject vs Props & Custom Events
---

* We should not always replace props & custom events with provide & inject
* Props & custom events should be the default communication method mostly
* We can consider using provide & inject whenever we have pass through components
* Problem with provide & inject is, it is hard to findout from which component the provide used and inject used.


---
# Section#9: Diving Deeper Into Components
---


---
## Section#9: 110. Global vs Local Components
---

* So far we registered our components with app like app.component('the-header', TheHeader);
    However, this is not always the best approach. And there otherways
* We did so far global component registration, that means when the app is loaded initially, all
    the registered components on the app shall be loaded initially and be made available to 
    through the app. However, this may not be a good practice if the project is huge and has lot
    of components. Out app/browser will end up loading all the components initially.
* So local components are nothing but, simply importing the required component inside a component,
    which is only being used in that component.
* Whenever we have a general purpose component which is referred commonly in most of the components, then
    better register them as global components.

---
## Section#9: 111. Scoped Styles
---

* No matter where we define the styles be it in vue app or components, they are bydefault treated as global.
    So, if you have styles defined in a component, it will effect for all the files.

* But if we want styles to scope only to those components, then we can use scoped attribute to the components style element

  eg: <style scoped>
      </style>

* Behind the scenes, vue generates an attribute and assign to those component specific elements, which in turn contains 
    those styles.

    eg: 
      <header data-v-9a9f6144>
      </header>

---
## Section#9: 112. Introducing Slots
---

* We can create a wrapper with a certain style attached (eg., BaseCard.vue) around dynamic content
* Its like a creating a css card style and passing the content to it dynamically, traditionally we would
    just add a class to html element to make it look like a card etc.,
* This is where we use slot
  <slot></slot>

* Basically props are meant for data, whereas slots are meant of getting html code/elements


---
## Section#9: 113. Named Slots
---

* When we use more than one slots in the component, then we can use names to identify the slot and pass accordingly
* You can have only one slot unnamed, that will be the default slot.
* template is the default html tag which does not render anything to the screen 

-- BaseCard.vue
<template>
    <div>
        <header>
            <slot name="header"></slot>
        </header>
        <slot></slot>
    </div>
</template>

<script>
</script>

<style scoped>
div {
  margin: 2rem auto;
  max-width: 30rem;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 1rem;
}
</style>

-- UserInfo.vue
<template>
  <section>
    <base-card>
      <template v-slot:header>
        <h3>{{ fullName }}</h3>
        <base-badge :type="role" :caption="role.toUpperCase()"></base-badge>
      </template>
      <p>{{ infoText }}</p>
    </base-card>
  </section>
</template>

<script>
export default {
  props: ['fullName', 'infoText', 'role'],
};
</script>

<style scoped>
section header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>

-- BadgeList.vue
<template>
  <section>
    <base-card>
      <template v-slot:header>
        <h2>Available Badges</h2>
      </template>

      <template v-slot:default>
        <ul>
          <li>
            <base-badge type="admin" caption="ADMIN"></base-badge>
          </li>
          <li>
            <base-badge type="author" caption="AUTHOR"></base-badge>
          </li>
        </ul>
      </template>
      </base-card>
  </section>
</template>


---
## Section#9: 114. Slot Styles and Compilation
---

* So when we are sending our html code to slots, it is disturbing the styles that are in the base component. Hence better move the styles
    also to the component with slot (eg. BaseCard)

---
## Section#9: 115. More on Slots
---

* We can also have default content rendered if a slot doesnt receive content.

-- BaseCard.vue
<template>
    <div>
        <header>
            <slot name="header">
                <h2>The Default Component</h2>
            </slot>
        </header>
        <slot></slot>
    </div>
</template>

* If the slot is empty and without a default content, it is rendered in DOM and can be viewed from console.
  However, if we want to get rid of that we can do as follows. In the following example, the header element
  doesnt get created if there is no header content passed.

-- BaseCard.vue
<template>
    <div>
        <header v-if="$slots.header">
            <slot name="header">
                <!-- <h2>The Default Component</h2> -->
            </slot>
        </header>
        <slot></slot>
    </div>
</template>

-- UserInfo.vue
<template>
  <section>
    <base-card>
      <template v-slot:header>
        <h3>{{ fullName }}</h3>
        <base-badge :type="role" :caption="role.toUpperCase()"></base-badge>
      </template>

      <template v-slot:default>
        <p>{{ infoText }}</p>
      </template>
    </base-card>
  </section>
</template>

<script>
export default {
  props: ['fullName', 'infoText', 'role'],
};
</script>

* Shortcut for passing elements to slots
  v-slot:header
  becomes
  #header


---
## Section#9: 116. Scoped Slots
---

* Sometimes, if we want a component to be available for team and customizable. The thing here is,
  what if the component that is using the slots have some dynamic data that needs to be used
  by the component sending the html content. This is where we can use scoped slots.

* The concept of scoped slots is about letting the component having slot to pass the data to
    component which is sending html content/markup. We could use props here like v-bind:item.
    After passing data from slot, the parent component needs to use template tag.


* This is more like sending data from slot to the component which is sending html content, in order
  for it to use the dynamic data


-- CourseGoals.vue
<template>
    <ul>
        <li v-for="goal in goals" :key="goal">
            <slot :item="goal" another-prop="..."></slot>
        </li>
    </ul>
</template>

<script>
export default {
    data() {
        return {
            goals: ['Finish the course', 'Leave Vue']
        };
    }
}
</script>

-- App.vue
<template>
  <div>
    <the-header></the-header>
    <badge-list></badge-list>
    <user-info
      :full-name="activeUser.name"
      :info-text="activeUser.description"
      :role="activeUser.role"
    ></user-info>
    <course-goals>
      <template #default="slotProps">
        <h2>{{ slotProps.item }}</h2>
        <p>{{ slotProps['anotherProp'] }}</p>
      </template>
    </course-goals>
  </div>
</template>

<script>
import TheHeader from './components/TheHeader.vue';
import BadgeList from './components/BadgeList.vue';
import UserInfo from './components/UserInfo.vue';
import CourseGoals from './components/CourseGoals.vue';

export default {
  components: {
    TheHeader,
    'badge-list': BadgeList,
    'user-info': UserInfo,
    'course-goals': CourseGoals
  },
  data() {
    return {
      activeUser: {
        name: 'Maximilian Schwarzmüller',
        description: 'Site owner and admin',
        role: 'admin',
      },
    };
  },
};
</script>

* If there is only one slot, we could avoid using template, so the above App.vue would look like this
-- App.vue
<template>
  <div>
    <the-header></the-header>
    <badge-list></badge-list>
    <user-info
      :full-name="activeUser.name"
      :info-text="activeUser.description"
      :role="activeUser.role"
    ></user-info>
    <course-goals #default="slotProps">
        <h2>{{ slotProps.item }}</h2>
        <p>{{ slotProps['anotherProp'] }}</p>
    </course-goals>
  </div>
</template>

<script>
import TheHeader from './components/TheHeader.vue';
import BadgeList from './components/BadgeList.vue';
import UserInfo from './components/UserInfo.vue';
import CourseGoals from './components/CourseGoals.vue';

export default {
  components: {
    TheHeader,
    'badge-list': BadgeList,
    'user-info': UserInfo,
    'course-goals': CourseGoals
  },
  data() {
    return {
      activeUser: {
        name: 'Maximilian Schwarzmüller',
        description: 'Site owner and admin',
        role: 'admin',
      },
    };
  },
};
</script>


---
## Section#9: 117. Dynamic Components
---

* If we want to dynamically show the components, basically conditionally showing the components
* We could do it simply by using v-if

-- App.vue
<template>
  <div>
    <the-header></the-header>
    <button @click="setSelectedComponent('active-goals')">Active Goals</button>
    <button @click="setSelectedComponent('manage-goals')">Manage Goals</button>
    <active-goals v-if="selectedComponent === 'active-goals'"></active-goals>
    <manage-goals v-if="selectedComponent === 'manage-goals'"></manage-goals>
  </div>
</template>

<script>
import TheHeader from './components/TheHeader.vue';
import ActiveGoals from './components/ActiveGoals.vue';
import ManageGoals from './components/ManageGoals.vue';

export default {
  components: {
    TheHeader,
    ActiveGoals,
    ManageGoals
  },
  data() {
    return {
      selectedComponent: 'active-goals',
      activeUser: {
        name: 'Maximilian Schwarzmüller',
        description: 'Site owner and admin',
        role: 'admin',
      },
    };
  },
  methods: {
    setSelectedComponent(cmp) {
      this.selectedComponent = cmp;
    }
  }
};
</script>

-- ActiveGoals.vue
<template>
    <h2>Active Goals</h2>
</template>

-- ManageGoals.vue
<template>
    <h2>Manage Goals</h2>
</template>

* But, when the code gets bigger, using v-if all the time is pretty annoying, thats why Vue has alternative here.
  This is called Dynamic Components.

  So the following code can be replaced with
  <active-goals v-if="selectedComponent === 'active-goals'"></active-goals>
  <manage-goals v-if="selectedComponent === 'manage-goals'"></manage-goals>

  replaced with

  <component :is="selectedComponent"></component>


---
## Section#9: 118. Keeping Dynamic Components Alive
---

* One additional thing we should know about Dynamic Components
* When we switch from manage goals after entering input field data to active goals, and then switch back to manage goals again,
    we can see that the data we intered in input field under manage goals disappeared. It is because, every time we switch the components
    using Dynamic Component, vue destroys and re-created the component in DOM. To avoid this, we could use keep-alive tag around 
    Dynamic Component like shown bellow.

-- App.vue
<template>
  <div>
    <the-header></the-header>
    <button @click="setSelectedComponent('active-goals')">Active Goals</button>
    <button @click="setSelectedComponent('manage-goals')">Manage Goals</button>

    <keep-alive>
      <component :is="selectedComponent"></component>
    </keep-alive>
  </div>
</template>  

---
## Section#9: 119. Applying What We Know & A Problem
---

* Using slots concept and show inputIsInvalid error popup dialog

-- ErrorAlert.vue
<template>
    <dialog open>
        <slot></slot>
    </dialog>
</template>

<style scoped>
dialog {
    margin: 0;
    position: fixed;
    top: 20vh;
    left: 30%;
    width: 40%;
    background-color: white;
    box-shadow: 0 2px 8px rgba(0,0,0,0.2);
    padding: 1rem;
}
</style>

-- ManageGoals.vue
<template>
    <div>
        <h2>Manage Goals</h2>
        <input type="text" ref="goal" />
        <button @click="setGoal">Set Goal</button>
        <error-alert v-if="inputIsInvalid">
            <h2>Input is invalid!</h2>
            <p>Please enter atleast a few characters...</p>
            <button @click="confirmError">Okay</button>
        </error-alert>
    </div>
</template>

<script>
import ErrorAlert from './ErrorAlert.vue';

export default {
    components: {
        ErrorAlert
    },
    data() {
        return {
            inputIsInvalid: false
        }
    },
    methods: {
        setGoal() {
            const enteredValue = this.$refs.goal.value;
            if (enteredValue === ''){
                this.inputIsInvalid = true;
            }
        },
        confirmError() {
            this.inputIsInvalid = false;
        }
    }
}
</script>

---
## Section#9: 120. Teleporting Elements
---

* In the previous example our Dialog popup is just below button in ManageGoals.vue, we can inspect in console and see the element.
  For accessibility and symantically correct html reasons, this is not a good practice. Instead we better keep these dialogues somewhere in root element.

* We can achieve this using a feature called Teleport. Teleport is a built-in Vue component just like <component> & <keep-alive>.
  We can wrap our to be teleported component inside <teleport> component. Teleport component needs one property, the CSS selector
  to where the component needs to be teleported. Here, we are teleporting the element to body element.

-- ManageGoals.vue
<template>
    <div>
        <h2>Manage Goals</h2>
        <input type="text" ref="goal" />
        <button @click="setGoal">Set Goal</button>
        <teleport to="body">
            <error-alert v-if="inputIsInvalid">
                <h2>Input is invalid!</h2>
                <p>Please enter atleast a few characters...</p>
                <button @click="confirmError">Okay</button>
            </error-alert>
        </teleport>
    </div>
</template>


---
## Section#9: 121. Working with Fragments
---

* In Vue 2, inside <template></template> element of the component, one root element needs to be there, and 
    all the elements should be inside the root element.

* However, in Vue 3, this restriction is removed. Meaning that we dont need to have root element. So this is called fragments.


---
## Section#9: 122. The Vue Style Guide
---

* Folder Structure & File names
* There is an official Vue style guide

---
## Section#9: 123. Moving to a different Folder Structure
---

* Create folders under components like UI etc.,


---
# Section#10: Course Project: The Learning Resources App
---


-- public/index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>

-- ./App.vue
<template>
    <the-header title="RememberMe"></the-header>
    <the-resources></the-resources>
</template>

<script>
import TheHeader from './components/layouts/TheHeader.vue';
import TheResources from './components/learning-resources/TheResources.vue';

export default {
  components: {
    TheHeader,
    TheResources,
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

* {
    box-sizing: border-box;
}

html {
    font-family: 'Roboto', sans-serif;
}

body {
    margin: 0;
}
</style>

-- ./main.js
import { createApp } from 'vue';
import App from './App.vue';
import BaseCard from './components/UI/BaseCard.vue';
import BaseButton from './components/UI/BaseButton.vue';
import BaseDialog from './components/UI/BaseDialog.vue';

const app = createApp(App);

app.component('base-card', BaseCard);
app.component('base-button', BaseButton);
app.component('base-dialog', BaseDialog);
app.mount('#app');

-- src/components/layouts/TheHeader.vue
<template>
    <header>
        <h1>{{  title }}</h1>
    </header>
</template>

<script>
export default {
    props: ['title']
}
</script>

<style scoped>
header {
    width: 100%;
    height: 5rem;
    background-color: #640032;
    display: flex;
    justify-content: center;
    align-items: center;
}

header h1 {
    color: white;
    margin: 0;
}
</style>

-- src/components/UI/BaseButton.vue
<template>
    <button :class="mode">
        <slot></slot>
    </button>
</template>

<script>
export default {
    props: ['mode']
}
</script>

<style scoped>
button {
    padding: 0.75rem 1.5rem;
    font-family: inherit;
    background-color: #3a0061;
    border: 1px solid #3a0061;
    color: white;
    cursor: pointer;
}

button:hover,
button:active {
    background-color: #270041;
    border-color: #270041;
}

.flat {
    background-color: transparent;
    color: #3a0061;
    border: none;
}

.flat:hover,
.flat:active {
    background-color: #edd2ff;
}
</style>

-- src/components/UI/BaseCard.vue
<template>
    <div>
        <slot></slot>
    </div>
</template>

<style scoped>
div {
    border-radius: 12px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
    padding: 1rem;
    margin: 2rem auto;
    max-width: 40rem;
}
</style>

-- src/components/UI/BaseDialog.vue
<template>
    <teleport to="body">
        <div id="dialog-bg" @click="$emit('close')"></div>
        <dialog open>
            <header>
                <slot name="header">
                    <h2>{{  title }}</h2>
                </slot>
            </header>
            <section>
                <slot></slot>
            </section>
            <menu>
                <slot name="actions">
                    <base-button @click="$emit('close')">Close</base-button>
                </slot>
            </menu>
        </dialog>
    </teleport>
</template>

<script>
export default {
    emits: ['close'],
    props: {
        title: {
            type: String,
            required: false
        }
    }
}
</script>

<style scoped>
#dialog-bg {
    position: fixed;
    top: 0;
    left: 0;
    height: 100vh;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.75);
    z-index: 10;
}

dialog {
    position: fixed;
    top: 20vh;
    left: 10%;
    width: 80%;
    z-index: 100;
    border-radius: 12px;
    border: none;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
    padding: 0;
    margin: 0;
    overflow: hidden;
}

header {
    background-color: #3a0061;
    color: white;
    width: 100%;
    padding: 1rem;
}

header h2 {
    margin: 0;
}

section {
    padding: 1rem;
}

menu {
    padding: 1rem;
    display: flex;
    justify-content: flex-end;
    margin: 0;
}

@media (min-width: 768px){
    dialog {
        left: calc(50% - 20rem);
        width: 40rem;
    }
}
</style>

-- src/components/learning-resources/AddResource.vue
<template>
    <base-card>
        <form @submit.prevent="submitData">
            <div class="form-control">
                <label for="title">Title</label>
                <input id="title" name="title" type="text" ref="titleInput"/>
            </div>
            <div class="form-control">
                <label for="description">Description</label>
                <textarea id="description" name="description" rows="3" ref="descInput"></textarea>
            </div>
            <div class="form-control">
                <label for="link">Link</label>
                <input id="link" name="link" type="url" ref="linkInput"/>
            </div>
            <div>
                <base-button type="submit">Add Resource</base-button>
            </div>
        </form>

        <base-dialog v-if="inputIsInvalid" title="Invalid Input" @close="confirmError">
            <template #default>
                <p>Unfortunately, at least one input value is invalid.</p>
                <p>Please check all inputs and make sure you enter at least a few characters into each input field.</p>
            </template>
            <template #actions>
                <base-button @click="confirmError">Okay</base-button>
            </template>
        </base-dialog>

    </base-card>
</template>

<script>
import BaseButton from '../UI/BaseButton.vue';
export default {
  components: { BaseButton },
    inject: ['addResource'],
    data() {
        return {
            inputIsInvalid: false
        }
    },
    methods: {
        submitData() {
            // this.$emit('create-resource', this.$refs.titleInput.value, this.$refs.descInput.value, this.$refs.linkInput.value);
            const enteredTitle = this.$refs.titleInput.value;
            const enteredDescription = this.$refs.descInput.value;
            const enteredURL = this.$refs.linkInput.value;

            if (enteredTitle == ''){
                this.inputIsInvalid = true;
            } else if (enteredDescription == '') {
                this.inputIsInvalid = true;
            } else if (enteredURL == ''){
                this.inputIsInvalid = true;
            } else {
                this.addResource(enteredTitle, enteredDescription, enteredURL);
            }

        },
        confirmError() {
            this.inputIsInvalid = false;
        }
    }
}
</script>

<style scoped>
.form-control {
    margin: 1rem 0;
}

label {
    font-weight: bold;
    display: block;
    margin-bottom: 0.5rem;
}

input,
textarea {
    display: block;
    width: 100%;
    font: inherit;
    padding: 0.15rem;
    border: 1px solid #ccc;
}

input:focus,
textarea:focus {
    outline: none;
    border-color: #3a0061;
    background-color: #f7ebff;
}
</style>

-- src/components/learning-resources/LearningResource.vue
<template>
    <base-card>
        <form @submit.prevent="submitData">
            <div class="form-control">
                <label for="title">Title</label>
                <input id="title" name="title" type="text" ref="titleInput"/>
            </div>
            <div class="form-control">
                <label for="description">Description</label>
                <textarea id="description" name="description" rows="3" ref="descInput"></textarea>
            </div>
            <div class="form-control">
                <label for="link">Link</label>
                <input id="link" name="link" type="url" ref="linkInput"/>
            </div>
            <div>
                <base-button type="submit">Add Resource</base-button>
            </div>
        </form>

        <base-dialog v-if="inputIsInvalid" title="Invalid Input" @close="confirmError">
            <template #default>
                <p>Unfortunately, at least one input value is invalid.</p>
                <p>Please check all inputs and make sure you enter at least a few characters into each input field.</p>
            </template>
            <template #actions>
                <base-button @click="confirmError">Okay</base-button>
            </template>
        </base-dialog>

    </base-card>
</template>

<script>
import BaseButton from '../UI/BaseButton.vue';
export default {
  components: { BaseButton },
    inject: ['addResource'],
    data() {
        return {
            inputIsInvalid: false
        }
    },
    methods: {
        submitData() {
            // this.$emit('create-resource', this.$refs.titleInput.value, this.$refs.descInput.value, this.$refs.linkInput.value);
            const enteredTitle = this.$refs.titleInput.value;
            const enteredDescription = this.$refs.descInput.value;
            const enteredURL = this.$refs.linkInput.value;

            if (enteredTitle == ''){
                this.inputIsInvalid = true;
            } else if (enteredDescription == '') {
                this.inputIsInvalid = true;
            } else if (enteredURL == ''){
                this.inputIsInvalid = true;
            } else {
                this.addResource(enteredTitle, enteredDescription, enteredURL);
            }

        },
        confirmError() {
            this.inputIsInvalid = false;
        }
    }
}
</script>

<style scoped>
.form-control {
    margin: 1rem 0;
}

label {
    font-weight: bold;
    display: block;
    margin-bottom: 0.5rem;
}

input,
textarea {
    display: block;
    width: 100%;
    font: inherit;
    padding: 0.15rem;
    border: 1px solid #ccc;
}

input:focus,
textarea:focus {
    outline: none;
    border-color: #3a0061;
    background-color: #f7ebff;
}
</style>

-- src/components/learning-resources/StoredResources.vue
<template>
  <ul>
    <learning-resource
      v-for="res in resources"
      :key="res.id"
      :id="res.id"
      :title="res.title"
      :description="res.description"
      :link="res.link"
    >
    </learning-resource>
  </ul>
</template>

<script>
import LearningResource from './LearningResource.vue';

export default {
    components: {
        LearningResource
    },
    inject: ['resources']
}
</script>

<style scoped>
ul {
    list-style: none;
    margin: 0;
    padding: 0;
    margin: auto;
    max-width: 40rem;
}
</style>

-- src/components/learning-resources/TheResources.vue
<template>
    <div>
    <base-card>
        <base-button 
            @click="setSelectedTab('stored-resources')"
            :mode="storedResButtonMode"
        >Stored Resources</base-button>
        <base-button 
            @click="setSelectedTab('add-resource')"
            :mode="addResButtonMode"
        >Add Resource</base-button>
    </base-card>
    <keep-alive>
        <component :is="selectedTab"></component>
    </keep-alive>
    </div>
</template>

<script>
import StoredResources from './StoredResources.vue';
import AddResource from './AddResource.vue';

export default {
    components: {
        StoredResources,
        AddResource
    },
    data() {
        return {
            selectedTab: 'stored-resources',
            storedResources: [
                {
                id: 'official-guide',
                title: 'Official Guide',
                description: 'The official Vue.js documentation.',
                link: 'https://vuejs.org',
                },
                {
                id: 'google',
                title: 'Google',
                description: 'Learn to google...',
                link: 'https://google.com',
                },
            ],
        }
    },
    provide() {
        return {
            resources: this.storedResources,
            addResource: this.addResource,
            deleteResource: this.removeResource
        }
    },
    computed: {
        storedResButtonMode() {
            return this.selectedTab === 'stored-resources' ? null : 'flat';
        },
        addResButtonMode() {
            return this.selectedTab === 'add-resource' ? null : 'flat';
        }
    },
    methods: {
        setSelectedTab(tab) {
            this.selectedTab = tab;
        },
        addResource(title, description, url){
            this.storedResources.unshift({
                id: new Date().toISOString(),
                title: title,
                description: description,
                link: url
            });
            this.selectedTab = 'stored-resources';
        },
        removeResource(resId) {
            // this.storedResources = this.storedResources.filter(res => res.id != resId);
            const resIndex = this.storedResources.findIndex(res => res.id === resId);
            this.storedResources.splice(resIndex, 1);
        }
    }
}
</script>



---
# Section#11: Forms
---


---
## Section#11: 141. v-model & Inputs
---

---
## Section#11: 142. Working with v-model Modifiers and Numbers
---

* v-model automatically renders numbers inside <input type="number"> fields as numbers. However, $refs renders the numbers as Strings.
    By default what is stored in the value is always a string.

* We can also force Vue to convert to number by using modifiers

  v-model.number = "userAge"

---
## Section#11: 142. v-model and Dropdowns
---

* v-model on select tag can be used same as input element

---
## Section#11: 143. Using v-model with Checkboxes & Radiobuttons
---

* Incase of checkboxes, we need to value parameter for each checkbox element. And the data property of v-model should be list.
* If there are multiple checkboxes for the same name then we get a list, if there is only one checkbox then we get 
    only one value true/false.

---
## Section#11: 144. Adding Basic Form Validation
---

* v-model is executed with every key press, however if we want to validate after the input is fully entered,
    we could use @blur="validateInput".

-- App.vue
<template>
  <the-form></the-form>
</template>

<script>
import TheForm from './components/TheForm.vue';

export default {
  components: {
    TheForm
  }  
}
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
  background-color: #292929;
}
</style>

-- TheForm.vue
<template>
  <form @submit.prevent="submitForm">
    <div class="form-control" :class="{invalid: userNameValidity === 'invalid'}">
      <label for="user-name">Your Name</label>
      <input id="user-name" name="user-name" type="text" v-model.trim="userName" @blur="validateInput"/>
      <p v-if="userNameValidity === 'invalid'">Please enter a valid user name!</p>
    </div>
    <div class="form-control">
      <label for="age">Your Age (Years)</label>
      <input id="age" name="age" type="number" v-model="userAge" />
    </div>
    <div class="form-control">
      <label for="referrer">How did you hear about us?</label>
      <select id="referrer" name="referrer" v-model="referrer">
        <option value="google">Google</option>
        <option value="wom">Word of mouth</option>
        <option value="newspaper">Newspaper</option>
      </select>
    </div>
    <div class="form-control">
      <h2>What are you interested in?</h2>
      <div>
        <input id="interest-news" name="interest" type="checkbox" value="news" v-model="interest"/>
        <label for="interest-news">News</label>
      </div>
      <div>
        <input id="interest-tutorials" name="interest" type="checkbox" value="tutorials" v-model="interest"/>
        <label for="interest-tutorials">Tutorials</label>
      </div>
      <div>
        <input id="interest-nothing" name="interest" type="checkbox" value="nothing" v-model="interest"/>
        <label for="interest-nothing">Nothing</label>
      </div>
    </div>
    <div class="form-control">
      <h2>How do you learn?</h2>
      <div>
        <input id="how-video" name="how" type="radio" value="video" v-model="how"/>
        <label for="how-video">Video Courses</label>
      </div>
      <div>
        <input id="how-blogs" name="how" type="radio" value="blogs" v-model="how"/>
        <label for="how-blogs">Blogs</label>
      </div>
      <div>
        <input id="how-other" name="how" type="radio" value="other" v-model="how"/>
        <label for="how-other">Other</label>
      </div>
    </div>
    <div>
      <button>Save Data</button>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
      userName: '',
      userAge: null,
      referrer: 'wom',
      interest: [],
      how: null,
      userNameValidity: 'pending'
    }
  },
  methods: {
    submitForm() {
      console.log('userName: ' + this.userName);
      this.userName = '';
      console.log('referrer: ' + this.referrer);
      console.log('interest: ' + this.interest);
      console.log('how: ' + this.how);
    },
    validateInput() {
      if (this.userName == '') {
        this.userNameValidity = 'invalid';
      } else {
        this.userNameValidity = 'valid';
      }
    }
  }
}
</script>

<style scoped>
form {
  margin: 2rem auto;
  max-width: 40rem;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 2rem;
  background-color: #ffffff;
}

.form-control {
  margin: 0.5rem 0;
}

label {
  font-weight: bold;
}

h2 {
  font-size: 1rem;
  margin: 0.5rem 0;
}

input,
select {
  display: block;
  width: 100%;
  font: inherit;
  margin-top: 0.5rem;
}

select {
  width: auto;
}

input[type='checkbox'],
input[type='radio'] {
  display: inline-block;
  width: auto;
  margin-right: 1rem;
}

input[type='checkbox'] + label,
input[type='radio'] + label {
  font-weight: normal;
}

button {
  font: inherit;
  border: 1px solid #0076bb;
  background-color: #0076bb;
  color: white;
  cursor: pointer;
  padding: 0.75rem 2rem;
  border-radius: 30px;
}

button:hover,
button:active {
  border-color: #002350;
  background-color: #002350;
}

.form-control.invalid input {
  border-color: red;
}

.form-control.invalid label {
  color: red;
}
</style>

-- main.js
import { createApp } from 'vue';

import App from './App.vue';

createApp(App).mount('#app');

---
## Section#11; 146. Building a Custom Control Component
---

* Custom control is nothing but our component only

* By default buttons inside a form will submit that form. If we want a button element not to trigger submit, we could use type

  eg: <button type="button">Test</button>
* Now, how do we submit this value to our form?

---
## Section#11: 147. Using v-model on Custom Components
---

* v-model can be used on a component similar to on an input element. There is a special prop that can be used on
    the component called props: ['modelValue'] and emit an event for that value.

-- main.js
import { createApp } from 'vue';

import App from './App.vue';

createApp(App).mount('#app');

-- App.vue
<template>
  <the-form></the-form>
</template>

<script>
import TheForm from './components/TheForm.vue';

export default {
  components: {
    TheForm
  }  
}
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
  background-color: #292929;
}
</style>

-- TheForm.vue
<template>
  <form @submit.prevent="submitForm">
    <div class="form-control" :class="{invalid: userNameValidity === 'invalid'}">
      <label for="user-name">Your Name</label>
      <input id="user-name" name="user-name" type="text" v-model.trim="userName" @blur="validateInput"/>
      <p v-if="userNameValidity === 'invalid'">Please enter a valid user name!</p>
    </div>
    <div class="form-control">
      <label for="age">Your Age (Years)</label>
      <input id="age" name="age" type="number" v-model="userAge" />
    </div>
    <div class="form-control">
      <label for="referrer">How did you hear about us?</label>
      <select id="referrer" name="referrer" v-model="referrer">
        <option value="google">Google</option>
        <option value="wom">Word of mouth</option>
        <option value="newspaper">Newspaper</option>
      </select>
    </div>
    <div class="form-control">
      <rating-control v-model="rating"></rating-control>
    </div>
    <div class="form-control">
      <h2>What are you interested in?</h2>
      <div>
        <input id="interest-news" name="interest" type="checkbox" value="news" v-model="interest"/>
        <label for="interest-news">News</label>
      </div>
      <div>
        <input id="interest-tutorials" name="interest" type="checkbox" value="tutorials" v-model="interest"/>
        <label for="interest-tutorials">Tutorials</label>
      </div>
      <div>
        <input id="interest-nothing" name="interest" type="checkbox" value="nothing" v-model="interest"/>
        <label for="interest-nothing">Nothing</label>
      </div>
    </div>
    <div class="form-control">
      <h2>How do you learn?</h2>
      <div>
        <input id="how-video" name="how" type="radio" value="video" v-model="how"/>
        <label for="how-video">Video Courses</label>
      </div>
      <div>
        <input id="how-blogs" name="how" type="radio" value="blogs" v-model="how"/>
        <label for="how-blogs">Blogs</label>
      </div>
      <div>
        <input id="how-other" name="how" type="radio" value="other" v-model="how"/>
        <label for="how-other">Other</label>
      </div>
    </div>
    <div>
      <button>Save Data</button>
    </div>
  </form>
</template>

<script>
import RatingControl from './RatingControl.vue';

export default {
  components: {
    RatingControl
  },
  data() {
    return {
      userName: '',
      userAge: null,
      referrer: 'wom',
      interest: [],
      how: null,
      userNameValidity: 'pending',
      rating: null
    }
  },
  methods: {
    submitForm() {
      console.log('userName: ' + this.userName);
      this.userName = '';
      console.log('referrer: ' + this.referrer);
      console.log('interest: ' + this.interest);
      console.log('how: ' + this.how);
      console.log('rating: ' + this.rating);
    },
    validateInput() {
      if (this.userName == '') {
        this.userNameValidity = 'invalid';
      } else {
        this.userNameValidity = 'valid';
      }
    }
  }
}
</script>

<style scoped>
form {
  margin: 2rem auto;
  max-width: 40rem;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 2rem;
  background-color: #ffffff;
}

.form-control {
  margin: 0.5rem 0;
}

label {
  font-weight: bold;
}

h2 {
  font-size: 1rem;
  margin: 0.5rem 0;
}

input,
select {
  display: block;
  width: 100%;
  font: inherit;
  margin-top: 0.5rem;
}

select {
  width: auto;
}

input[type='checkbox'],
input[type='radio'] {
  display: inline-block;
  width: auto;
  margin-right: 1rem;
}

input[type='checkbox'] + label,
input[type='radio'] + label {
  font-weight: normal;
}

button {
  font: inherit;
  border: 1px solid #0076bb;
  background-color: #0076bb;
  color: white;
  cursor: pointer;
  padding: 0.75rem 2rem;
  border-radius: 30px;
}

button:hover,
button:active {
  border-color: #002350;
  background-color: #002350;
}

.form-control.invalid input {
  border-color: red;
}

.form-control.invalid label {
  color: red;
}
</style>

-- RatingControl.vue
-- Resetting the component value to null after submitForm is taken care
<template>
    <ul>
        <li :class="{active: modelValue === 'poor'}"><button type="button" @click="activate('poor')">Poor</button></li>
        <li :class="{active: modelValue === 'average'}"><button type="button" @click="activate('average')">Average</button></li>
        <li :class="{active: modelValue === 'great'}"><button type="button" @click="activate('great')">Great</button></li>
    </ul>
</template>

<script>
export default {
    props: ['modelValue'],
    emits: ['update:modelValue'],
    // PROBLEM: After submit this is not getting reset
    // data() {
    //     return {
    //         activeOption: this.modelValue
    //     }
    // },

    // OR: We can skip this all togher and directly put the value in li
    // computed: {
    //     activeOption() {
    //         return this.modelValue;
    //     }
    // },
    methods: {
        activate(option) {
            // this.activeOption = option;
            this.$emit('update:modelValue', option);
        }
    }
}
</script>

<style scoped>
.active {
    border-color: #a00078;
}

.active button {
    color: #a00078;
}

ul {
    list-style: none;
    margin: 0.5rem 0;
    padding: 0;
    display: flex;
}

li {
    margin: 0 1rem;
    border: 1px solid #ccc;
    padding: 1rem;
    display: flex;
    justify-content: center;
    align-items: center;
}

button {
    font: inherit;
    border: none;
    background-color: transparent;
    cursor: pointer;
}
</style>


---
# Section#12: Sending Http Requests
---


---
## Section#12: 152. Adding a Backend
---

* Here instead of making PHP, Node.js backend scripts, we are going to use Firebase for the purpose of practicing.
* Firebase is a service that gives us a backend where we dont have to write any code to get started with storing data.
    Its like the Firebase is already pre-writted and pre-configured the backend for you.
* We will use Realtime Database option, this gives us nice REST API.

---
## Section#12: 153. How to (Not) Send Http Requests
---

* My Firebase database url
https://vue-http-demo-d0c48-default-rtdb.firebaseio.com/

* We can use third party packages like axios, which is a very popular javascript package for sending Http requests
    from inside javascript.

* Alternatively browsers also have built-in methods for sending Http requests. We can use fetch() method to get and
    send data as well. Fetch takes an URL as first argument, and then we can configure it to fetch/send data to that URL.


---
## Section#12: 154. Sending a POST Request to Store Data
---

* We send the data as json which is a firebase requirement.
  fetch('https://vue-http-demo-d0c48-default-rtdb.firebaseio.com/surveys.json')
* Here the name can be anything et., surveys. Firebase creates a node with that name and saves data there.
    Be default, it gets data. We can add a second argument(javascript object) for sending data.

* We can use JSON.stringify({name: 'Hello'}) to convert javascript object to JSON.

  Example:

  fetch('https://vue-http-demo-d0c48-default-rtdb.firebaseio.com/surveys.json', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      name: this.enteredName,
      rating: this.chosenRating
    }),
  });

---
## Section#12: 155. Http Requests & Http Methods (Verbs)
---

Http Requests & Http Methods (Verbs)
In the last lecture, we sent a POST request to a REST API.

What is that? A POST request? And a REST API?

There are different "kinds" of Http requests you could say - defined by the method (POST, GET, DELETE, ...) you attach to them (via the "method" you define on an outgoing request).

And the server to which you are sending those requests may then react in which ever way it is configured to react to incoming requests with different methods.

It may store data in a database for an incoming POST request, it may fetch data for a GET request.

Typically, servers are built to work as a "REST API" - that means they have clearly defined "endpoints" (URL + Http method combinations) for which they do different things.

You can learn more about REST APIs (and how to build your own one!) with this free series: https://academind.com/learn/node-js/building-a-restful-api-with/what-is-a-restful-api-/

Also make sure you understand, in general, how the web works: https://academind.com/learn/web-dev/how-the-web-works/

-- Building a RESTful API
https://academind.com/tutorials/building-a-restful-api-with-nodejs

-- Understand how the Web works
https://academind.com/tutorials/

---
## Section#12: 156. Using Axios Instead Of "fetch()"
---

Using Axios Instead Of "fetch()"
In this course, we use the native fetch() API for sending Http requests. It's built into the browser and hence we don't have to install any extra package to use it.

If you prefer third-party libraries like Axios (https://github.com/axios/axios) you can of course also use such libraries though.

For example, you could replace the fetch() code from the last lecture with this Axios code:

Instead of:

fetch('https://vue-http-demo-85e9e.firebaseio.com/surveys.json', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    name: this.enteredName,
    rating: this.chosenRating,
  }),
});
you can write this code with Axios:

import axios from 'axios'; // at the start of your <script> tag, before you "export default ..."
...
axios.post('https://vue-http-demo-85e9e.firebaseio.com/surveys.json', {
  name: this.enteredName,
  rating: this.chosenRating,
});
As you can see, with Axios, you have to write less code. It automatically sets the Content-Type header for you, it also automatically converts the body data to JSON.

BUT - as a downside - you have to add an extra package, which ultimately increases the size of the web app you're shipping in the end.

Later in the module, we'll also start reacting to the response returned by the request.

fetch() returns a Promise, hence we can use then(), catch() and async/ await there. For Axios, it is just the same - it also returns a Promise.

---
## Section#12: 157. Getting Data (GET Request) & Transforming Response Data
---

* fetch() returns an object on which we can listen for data once it is there, to then setup code which will be executed when that data is there.
* We setup such listender by sing then().
* Firebase sends data back in json format
* Now here response.json() also yields a promise, so we use the sencond then() again
* The arrow function is same like regular function, except that the this key word works the same as outside the block. With regular function,
    we get the following error, it does not refer to the vue instance object.

  ERROR-:
  erExperiences.vue:44 Uncaught (in promise) TypeError: Cannot set properties of undefined (setting 'results')
    at eval (UserExperiences.vue:44:1)


-- UserExperiences.vue
  methods: {
    loadExperiences() {
      fetch(
        'https://vue-http-demo-d0c48-default-rtdb.firebaseio.com/surveys.json'
      )
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
          const results = [];
          for (const id in data) {
            results.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating,
            });
          }
          this.results = results;
        });
    },
  },

---
## Section#12: 158. Loading Data When a Component Mounts
---

* Lets display the data when we reload the page. We can simply do this by using mounted method on the component and call the respective
    method to load the data.

-- UserExperiences.vue
<script>
import SurveyResult from './SurveyResult.vue';

export default {
  // props: ['results'],
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
    };
  },
  methods: {
    loadExperiences() {
      fetch(
        'https://vue-http-demo-d0c48-default-rtdb.firebaseio.com/surveys.json'
      )
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
          const results = [];
          for (const id in data) {
            results.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating,
            });
          }
          this.results = results;
        });
    },
  },
  mounted() {
    this.loadExperiences();
  }
};
</script>


---
## Section#12: 159. Showing a "Loading..." Message
---

-- UserExperiences.vue
    <p v-if="isLoading">Loading...</p>
    <ul v-else>
      <survey-result
        v-for="result in results"
        :key="result.id"
        :name="result.name"
        :rating="result.rating"
      ></survey-result>

---
## Section#12: 160. Handling the "No Data" state
---

-- UserExperiences.vue
  <p v-if="isLoading">Loading...</p>
  <p v-else-if="!isLoading && (!results || results.length === 0)">No stored experiences found. Start adding some survey results first.</p>
  <ul v-else-if="!isLoading && results && results.length > 0">
    <survey-result
      v-for="result in results"
      :key="result.id"
      :name="result.name"
      :rating="result.rating"
    ></survey-result>
  </ul>

---
## Section#12: 161. Handling Technical / Browser-side Errors
---

* We can listen for technical errors like wrong url etc., using .catch()

-- UserExperiences.vue
    <p v-if="isLoading">Loading...</p>
    <p v-else-if="!isLoading && error">
      {{ error }}
    </p>
    <p v-else-if="!isLoading && (!results || results.length === 0)">No stored experiences found. Start adding some survey results first.</p>
    <ul v-else>
      <survey-result
        v-for="result in results"
        :key="result.id"
        :name="result.name"
        :rating="result.rating"
      ></survey-result>
    </ul>

  methods: {
    loadExperiences() {
      this.isLoading = true;
      this.error = null;
      fetch(
        'https://vue-http-demo-d0c48-default-rtdb.firebaseio.com/surveys.json'
      )
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
          this.isLoading = false;
          const results = [];
          for (const id in data) {
            results.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating,
            });
          }
          this.results = results;
        })
        .catch((error) => {
          console.log(error);
          this.error = 'Failed to fetch data - please try again later.';
          // this.error = error;
          this.isLoading = false;
        });
    },
  },

---
## Section#12: 162. Handling Error Responses
---

* Technical errors like missing json in the url above, are handled via .catch(). 
* If there are errors from server side or firebase side which are not technical errors
* For example, lets say the url is correct with .json extension, but the body data is without JSON.stringify,
    this will result in "400 (Bad Request)" error, but will not be caught by .catch() method because it catches
    only technical issues.
* So, here we can get the error from .then(response) itself if the response.ok is not met.

---
## Section#12: 163. Module Summary
---

-- index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>

-- main.js
import { createApp } from 'vue';

import BaseCard from './components/UI/BaseCard.vue';
import BaseButton from './components/UI/BaseButton.vue';
import App from './App.vue';

const app = createApp(App);

app.component('base-card', BaseCard);
app.component('base-button', BaseButton);

app.mount('#app');

-- App.vue
<template>
  <learning-survey></learning-survey>
  <user-experiences></user-experiences>
</template>

<script>
import LearningSurvey from './components/survey/LearningSurvey.vue';
import UserExperiences from './components/survey/UserExperiences.vue';

export default {
  components: {
    LearningSurvey,
    UserExperiences,
  },
  // data() {
  //   return {
  //     savedSurveyResults: [],
  //   };
  // },
  // methods: {
  //   storeSurvey(surveyData) {
  //     const surveyResult = {
  //       name: surveyData.userName,
  //       rating: surveyData.rating,
  //       id: new Date().toISOString(),
  //     };
  //     this.savedSurveyResults.push(surveyResult);
  //     console.log(surveyResult);
  //   },
  // },
};
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}
</style>

-- BaseButton.vue
<template>
  <button>
    <slot></slot>
  </button>
</template>

<style scoped>
button {
  font: inherit;
  border: 1px solid #360032;
  background-color: #360032;
  color: white;
  padding: 0.5rem 2rem;
  cursor: pointer;
}

button:hover,
button:active {
  background-color: #5c0556;
  border-color: #5c0556;
}
</style>

-- BaseCard.vue
<template>
  <div>
    <slot></slot>
  </div>
</template>

<style scoped>
div {
  margin: 2rem auto;
  max-width: 40rem;
  padding: 1rem;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
}
</style>

-- LearningSurvey.vue
<template>
  <section>
    <base-card>
      <h2>How was you learning experience?</h2>
      <form @submit.prevent="submitSurvey">
        <div class="form-control">
          <label for="name">Your Name</label>
          <input type="text" id="name" name="name" v-model.trim="enteredName" />
        </div>
        <h3>My learning experience was ...</h3>
        <div class="form-control">
          <input type="radio" id="rating-poor" value="poor" name="rating" v-model="chosenRating" />
          <label for="rating-poor">Poor</label>
        </div>
        <div class="form-control">
          <input
            type="radio"
            id="rating-average"
            value="average"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-average">Average</label>
        </div>
        <div class="form-control">
          <input type="radio" id="rating-great" value="great" name="rating" v-model="chosenRating" />
          <label for="rating-great">Great</label>
        </div>
        <p
          v-if="invalidInput"
        >One or more input fields are invalid. Please check your provided data.</p>
        <p v-if="error">{{ error }}</p>
        <div>
          <base-button>Submit</base-button>
        </div>
      </form>
    </base-card>
  </section>
</template>

<script>
export default {
  data() {
    return {
      enteredName: '',
      chosenRating: null,
      invalidInput: false,
      error: null
    };
  },
  // emits: ['survey-submit'],
  methods: {
    submitSurvey() {
      if (this.enteredName === '' || !this.chosenRating) {
        this.invalidInput = true;
        return;
      }
      this.invalidInput = false;

      // this.$emit('survey-submit', {
      //   userName: this.enteredName,
      //   rating: this.chosenRating,
      // });

      this.error = null;
      fetch('https://vue-http-demo-d0c48-default-rtdb.firebaseio.com/surveys.json', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          name: this.enteredName,
          rating: this.chosenRating
        }),
      })
      .then(response => {
        if (response.ok) {
          // ....
        } else {
          // this.error = 'This is the error from server side...';
          throw new Error('Could not save data!');
        }
      })
      .catch((error) => {
        console.log(error);
        // this.error = 'Something went wrong - try again later!';
        this.error = error.message;
      });

      this.enteredName = '';
      this.chosenRating = null;
    },
  },
};
</script>

<style scoped>
.form-control {
  margin: 0.5rem 0;
}

input[type='text'] {
  display: block;
  width: 20rem;
  margin-top: 0.5rem;
}
</style>

-- SurveyResult.vue
<template>
  <li>
    <p>
      <span class="highlight">{{ name }}</span> rated the learning experience
      <span :class="ratingClass">{{ rating }}</span>.
    </p>
  </li>
</template>

<script>
export default {
  props: ['name', 'rating'],
  computed: {
    ratingClass() {
      return 'highlight rating--' + this.rating;
    },
  },
};
</script>

<style scoped>
li {
  margin: 1rem 0;
  border: 1px solid #ccc;
  padding: 1rem;
}

h3,
p {
  font-size: 1rem;
  margin: 0.5rem 0;
}

.highlight {
  font-weight: bold;
}

.rating--poor {
  color: #b80056;
}

.rating--average {
  color: #330075;
}

.rating--great {
  color: #008327;
}
</style>

-- UserExperiences.vue
<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="!isLoading && error">
        {{ error }}
      </p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">No stored experiences found. Start adding some survey results first.</p>
      <ul v-else>
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  // props: ['results'],
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoading: false,
      error: null
    };
  },
  methods: {
    loadExperiences() {
      this.isLoading = true;
      this.error = null;
      fetch(
        'https://vue-http-demo-d0c48-default-rtdb.firebaseio.com/surveys.json'
      )
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
          this.isLoading = false;
          const results = [];
          for (const id in data) {
            results.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating,
            });
          }
          this.results = results;
        })
        .catch((error) => {
          console.log(error);
          this.error = 'Failed to fetch data - please try again later.';
          // this.error = error;
          this.isLoading = false;
        });
    },
  },
  mounted() {
    this.loadExperiences();
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>



---
# Section#13: Routing: Building "Multi Page" Single Page Applications
---



---
## Section#13: 166. What & Why ?
---

* Single Page Application, is like the url is generated from javascript itself like in frappe doctype list view etc., so that
    the url can be shared or linked. So typically, we will have only one html and then the URLs and routing is taken care
    accordingly.

---
## Section#13: 167. Routing Setup
---

-- Installing router package

npm install --save vue-router

const router = createRouter({
    history: ,
    routes: []
});

* routes, list of routes that we want to support in the app.
* history tells, how to mange the routing history in this app. There are two different kinds of history we can use here. 
    Historically javascript in the browser wasn't always able to manipulate browser's memory on which page the user is on
    and which page the user came from. Therefore, historically the router has to emulate this behavior. And we didn't use 
    the built-in browser history. So with createWebHistory, we can tell the app to use the browser's built in mechanism.

    * createWebHistory


---
## Section#13: 168. Registering & Rendering Routes
---

* We can define the routs in main.js, and replace the components rendering in App.vue vith router-view component.

-- main.js
import { createApp } from 'vue';
import { createRouter, createWebHistory } from 'vue-router';

import App from './App.vue';
import TeamsList from './components/teams/TeamsList.vue';
import UsersList from './components/users/UsersList.vue';

const router = createRouter({
    history: createWebHistory(),
    routes: [
        {path: '/teams', component: TeamsList},
        {path: '/users', component: UsersList}
    ]
});

const app = createApp(App)

app.use(router);

app.mount('#app');

-- App.vue
<template>
  <the-navigation @set-page="setActivePage"></the-navigation>
  <main>
    <router-view></router-view>
  </main>
</template>

<script>
import TheNavigation from './components/nav/TheNavigation.vue';

export default {
  components: {
    TheNavigation,
  },
  data() {
    return {
      activePage: 'teams-list',
      teams: [
        { id: 't1', name: 'Frontend Engineers', members: ['u1', 'u2'] },
        { id: 't2', name: 'Backend Engineers', members: ['u1', 'u2', 'u3'] },
        { id: 't3', name: 'Client Consulting', members: ['u4', 'u5'] },
      ],
      users: [
        { id: 'u1', fullName: 'Max Schwarz', role: 'Engineer' },
        { id: 'u2', fullName: 'Praveen Kumar', role: 'Engineer' },
        { id: 'u3', fullName: 'Julie Jones', role: 'Engineer' },
        { id: 'u4', fullName: 'Alex Blackfield', role: 'Consultant' },
        { id: 'u5', fullName: 'Marie Smith', role: 'Consultant' },
      ],
    };
  },
  provide() {
    return {
      teams: this.teams,
      users: this.users,
    };
  },
  methods: {
    setActivePage(page) {
      this.activePage = page;
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}
</style>


---
## Section#13: 169. Navigating with router-link
---

* Now instead of button, we can use rounter-link to navigate to the pages. This is like a special anchor tag which will not
    load a different page and reload the entire App. So this special anchor tag prevents the default browser reload,
    and then Vue takes over and loads the appropriate component and updates the URL.

* One thing to remember, there are two classes added on the active router-link
    * router-link-active
    * router-link-exact-active

---
## Section#13: 170. Styling Active Links
---

* We can apply styles for router-link-active and router-link-exact-active
* router-link-active would also be applied on nested routing, meaning additional routing on the main link
* router-link-exact-active will be applied only in the page
* We can also change those default classes which are defined in the component by specifying at the time of router creation in main.js
    - linkActiveClass: 'active'
    - linkExactActiveClass: 'somethingelse'

-- main.js
const router = createRouter({
    history: createWebHistory(),
    routes: [
        {path: '/teams', component: TeamsList},
        {path: '/users', component: UsersList}
    ],
    linkActiveClass: 'active',
    linkExactActiveClass: 'inactive'
});


---
## Section#13: 171. Programmatic Navigation
---

* Sometimes we may have to navigate programmatically. We could do this by using this.$router which is available if our app is using router package.
    Similarly we can also use this.$router.back(), this.$router.forward() etc.,  
  Example:
    this.$router.push('/teams');

-- UsersList.vue
<script>
import UserItem from './UserItem.vue';

export default {
  components: {
    UserItem,
  },
  inject: ['users'],
  methods: {
    confirmInput() {
      this.$router.push('/teams');
    }
  }
};
</script>

---
## Section#13: 172. Passing Data with Route Params (Dynamic Segments)
---

* Dynamic route

-- main.js
const router = createRouter({
    history: createWebHistory(),
    routes: [
        {path: '/teams', component: TeamsList},
        {path: '/users', component: UsersList},
        {path: '/teams/:teamId', component: TeamMembers}
    ],
    // linkActiveClass: 'active',
    // linkExactActiveClass: 'inactive'
});

* Getting access to dynamic segment /teams/:teamId user entered. So we can write our code in .created() method which is called before 
    the app is visible, we can use this.route (not to get confused with this.router) to refer to the current route which gives
    additional options to access several information from the current route. Here we could use this.$route.params to get router paramters.

-- TeamMembers.vue
<script>
import UserItem from '../users/UserItem.vue';

export default {
  inject: ['users', 'teams'],
  components: {
    UserItem
  },
  data() {
    return {
      teamName: '',
      members: []
    };
  },
  created() {
    const teamId = this.$route.params.teamId;
    const selectedTeam = this.teams.find(team => team.id === teamId);
    const members = selectedTeam.members;
    const selectedMembers = [];
    for (const member of members) {
      const selectedUser = this.users.find(user => user.id == member);
      selectedMembers.push(selectedUser);
    }
    this.members = selectedMembers;
    this.teamName= selectedTeam.name;
  }
};
</script>


---
## Section#13: 173. Navigation & Dynamic Paths
---

* Now lets make the "View Members" button in teams page to show these members

-- TeamsItem.vue
<template>
  <li>
    <h3>{{ name }}</h3>
    <div class="team-members">{{ memberCount }} Members</div>
    <router-link :to="teamMembersLink">View Members</router-link>
  </li>
</template>

<script>
export default {
  props: ['id', 'name', 'memberCount'],
  computed: {
    teamMembersLink() {
      return '/teams/' + this.id;
    }
  }
};
</script>

---
## Section#13: 174. A Vue Bug
---

A Vue Bug
In the next lecture, you might encounter an error when following along.

Uncaught (in promise) TypeError: Cannot read property 'members' of undefined

This error occurs because of a bug with Vue.js (NOT because the code would be wrong). 
Fixing it is easy - have a look at this Q&A thread if you're running into the 
problem: https://www.udemy.com/course/vuejs-2-the-complete-guide/learn/#questions/12619882/

'
---
## Section#13: 175. Updating Params Data with Watchers
--- 

* If you are on the page that was loaded for a dynamic paramter, and then if you wanna go to a different page with a different values
    for this dynamic paramter which is basically on the same component we face the issue. This is basically not a bug, the vue
    router does not destroy and rebuild the component for performance reasons.

* PROBLEM: Suppose, if we are already on /teams/t1 page and if there is any link like to /teams/t2 from within the t1 page.
    Clicking on the /teams/t2 link changes in browser url but it doesnt reload the page.
    That t2 link doesnt work, since t1 and t2 are within the same route /teams, Vue js tries to show the link without loading the page,
    but here when we first went to /teams/t1, the object was already created via .created() method, however it will not create again
    when we call /teams/t2 since the page is not reloading. 


* So, one work around is. Whenever the url changes the $router also changes, so we can have a watch on this and call our methods
    to list the data accordingly.

-- TeamMembers.vue
<script>
import UserItem from '../users/UserItem.vue';

export default {
  inject: ['users', 'teams'],
  components: {
    UserItem
  },
  data() {
    return {
      teamName: '',
      members: []
    };
  },
  methods: {
    loadTeamMembers(route) {
      const teamId = route.params.teamId;
      const selectedTeam = this.teams.find(team => team.id === teamId);
      const members = selectedTeam.members;
      const selectedMembers = [];
      for (const member of members) {
        const selectedUser = this.users.find(user => user.id == member);
        selectedMembers.push(selectedUser);
      }
      this.members = selectedMembers;
      this.teamName= selectedTeam.name;
    }
  },
  created() {
    this.loadTeamMembers(this.$route);
  },
  watch: {
    $route(newRoute) {
      this.loadTeamMembers(newRoute);
    }
  }
};
</script>


---
## Section#13: 176. Passing Params as Props
--- 

* So, there is an issue with the above approach. It is simply not scalable if we want to embded the above component
    somewhere else since it depends on $route. However, we can make it reusual with the following approach using props

* Router doesnt pass any props by default, however we can configure that in main.js to send the params as props.

  Example:-
    {path: '/teams/:teamId', component: TeamMembers, props: true}

-- main.js
const router = createRouter({
    history: createWebHistory(),
    routes: [
        {path: '/teams', component: TeamsList},
        {path: '/users', component: UsersList},
        {path: '/teams/:teamId', component: TeamMembers, props: true}
    ],
    // linkActiveClass: 'active',
    // linkExactActiveClass: 'inactive'
});


-- TeamMembers.vue
export default {
  props: ['teamId'],
  inject: ['users', 'teams'],
  components: {
    UserItem
  },
  data() {
    return {
      teamName: '',
      members: []
    };
  },
  methods: {
    loadTeamMembers(teamId) {
      const selectedTeam = this.teams.find(team => team.id === teamId);
      const members = selectedTeam.members;
      const selectedMembers = [];
      for (const member of members) {
        const selectedUser = this.users.find(user => user.id == member);
        selectedMembers.push(selectedUser);
      }
      this.members = selectedMembers;
      this.teamName= selectedTeam.name;
    }
  },
  created() {
    this.loadTeamMembers(this.teamId);
  },
  watch: {
    teamId(newId) {
      this.loadTeamMembers(newId);
    }
  }
};
</script>

---
## Section#13: 177. Redirecting & "Catch All" Routes
--- 

* We can set route for home route, and also set a route for everything else if the url doesnt exist.
    We can make these changes in main.js.

  We can do either:

    {path: '/', component: TeamsList},
  
    OR (redirect changes the url to /teams)
  
    {path: '/', redirect: '/teams'},
  
    OR (alias will not change the url to /teams, instead it will be at / only)
  
    {path: '/teams', component: TeamsList, alias: '/'},

* Catch All, here dynamic segment name can be anything notFound, catchAll etc.,

  {path: '/:notFound(.*)', component: NotFound}

-- main.js
import { createApp } from 'vue';
import { createRouter, createWebHistory } from 'vue-router';

import App from './App.vue';
import TeamsList from './components/teams/TeamsList.vue';
import UsersList from './components/users/UsersList.vue';
import TeamMembers from './components/teams/TeamMembers.vue';
import NotFound from './components/nav/NotFound.vue';

const router = createRouter({
    history: createWebHistory(),
    routes: [
        {path: '/', redirect: '/teams'},
        // {path: '/teams', component: TeamsList, alias: '/'},
        {path: '/teams', component: TeamsList},
        {path: '/users', component: UsersList},
        {path: '/teams/:teamId', component: TeamMembers, props: true},
        {path: '/:notFound(.*)', component: NotFound}
    ],
    // linkActiveClass: 'active',
    // linkExactActiveClass: 'inactive'
});

const app = createApp(App)

app.use(router);

app.mount('#app');


-- NotFound.vue
<template>
    <h2>Page not found! Maybe view our <router-link to="/teams">Teams</router-link>?</h2>
</template>


---
## Section#13: 178. Nested Routes
--- 

* A router inside another router
* Suppose we want to render the team members, within the teams list page

-- main.js

* here the /teams/:teamId is moved inside /teams

const router = createRouter({
    history: createWebHistory(),
    routes: [
        {path: '/', redirect: '/teams'},
        {path: '/teams', component: TeamsList, children: [
            {path: ':teamId', component: TeamMembers, props: true},
        ]},
        {path: '/users', component: UsersList},
        {path: '/:notFound(.*)', component: NotFound}
    ],
});

-- TeamsList.vue

* Since we moved the route from root level to inside /teams, now the system doesn't know where to render. Hence
    we can add <router-view></router-view> to TeamsList to render it there'

<template>
  <router-view></router-view>
  <ul>
    <teams-item
      v-for="team in teams"
      :key="team.id"
      :id="team.id"
      :name="team.name"
      :member-count="team.members.length"
    ></teams-item>
  </ul>
</template>


---
## Section#13: 179. More Fun with Named Routes & Location Objects
--- 

* If there are lot of nested routes. Constructing links like "return '/teams/' + this.id; " becomes very cumbersome.
    Vue provides a better option for this purpose. So instead of having lot of url constructs with / , we could simply
    name our route and use it.

-- main.js
* Using name property to assign names to urls/routes

    {
      name: 'teams',
      path: '/teams',
      component: TeamsList,
      children: [{ name: 'team-members', path: ':teamId', component: TeamMembers, props: true }],
    },

-- TeamsItem.vue
* We could change the existing link code to 

<script>
export default {
  props: ['id', 'name', 'memberCount'],
  computed: {
    teamMembersLink() {
      // return '/teams/' + this.id;
      return { name: 'team-members', params: { teamId: this.id }};
    }
  }
};
</script>

---
## Section#13: 180. Using Query Params
--- 

* Query parameters are usually passed via URL after ? mark
* We could use query parameters as shown below

-- TeamsItem.vue
<template>
  <li>
    <h3>{{ name }}</h3>
    <div class="team-members">{{ memberCount }} Members</div>
    <router-link :to="teamMembersLink">View Members</router-link>
  </li>
</template>

<script>
export default {
  props: ['id', 'name', 'memberCount'],
  computed: {
    teamMembersLink() {
      // return '/teams/' + this.id;
      return {
        name: 'team-members',
        params: { teamId: this.id },
        query: { sort: 'asc' },
      };
    },
  },
};
</script>

So when we click on the router-link, it makes the url to 

http://localhost:8080/teams/t3?sort=asc

* And then we can use this.$route.query to get the query parameters in the called page

-- TeamMembers.vue

---
## Section#13: 181. Rendering Multiple Routes with Named Router Views
--- 

* We can have Multiple router views on the same level/component. So in our main.js instead of one component, we can register multiple components
    and give them names.

-- main.js
import { createApp } from 'vue';
import { createRouter, createWebHistory } from 'vue-router';

import App from './App.vue';
import TeamsList from './components/teams/TeamsList.vue';
import UsersList from './components/users/UsersList.vue';
import TeamMembers from './components/teams/TeamMembers.vue';
import NotFound from './components/nav/NotFound.vue';
import TeamsFooter from './components/teams/TeamsFooter.vue';
import UsersFooter from './components/users/UsersFooter.vue';

const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: '/', redirect: '/teams' },
    // {path: '/teams', component: TeamsList, alias: '/'},
    {
      name: 'teams',
      path: '/teams',
      components: { default: TeamsList, footer: TeamsFooter },
      children: [
        {
          name: 'team-members',
          path: ':teamId',
          component: TeamMembers,
          props: true,
        },
      ],
    },
    { path: '/users', components: { default: UsersList, footer: UsersFooter } },
    { path: '/:notFound(.*)', component: NotFound },
  ],
  // linkActiveClass: 'active',
  // linkExactActiveClass: 'inactive'
});

const app = createApp(App);

app.use(router);

app.mount('#app');

-- App.vue
<template>
  <the-navigation></the-navigation>
  <main>
    <router-view></router-view>
  </main>
  <footer>
    <router-view name="footer"></router-view>
  </footer>
</template>

-- TeamsFooter.vue
<template>
    <h2>Teams Footer</h2>
</template>

-- UsersFooter.vue
<template>
    <h2>Users Footer</h2>
</template>



---
## Section#13: 182. Controlling Scroll Behavior
--- 

* Suppose after listing the team members if we want scroll to top of the page automatically. We can do this
    by adding additional configuration in our main.js called scrollBehavior(). This method receives 3 argments automatically.

    scrollBehavior(to, from, savedPosition)
      to: page we are going
      from: page we are coming from
      savedPosition: object {left: 0, top: 0}, is the previous page position from where we moved to new page,
        this helps us go back to the exact position when we click on previous link
  * we can also access to, from using this.$route

-- main.js

const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: '/', redirect: '/teams' },
    // {path: '/teams', component: TeamsList, alias: '/'},
    {
      name: 'teams',
      path: '/teams',
      components: { default: TeamsList, footer: TeamsFooter },
      children: [
        {
          name: 'team-members',
          path: ':teamId',
          component: TeamMembers,
          props: true,
        },
      ],
    },
    { path: '/users', components: { default: UsersList, footer: UsersFooter } },
    { path: '/:notFound(.*)', component: NotFound },
  ],
  // linkActiveClass: 'active',
  // linkExactActiveClass: 'inactive',
  scrollBehavior(to, from, savedPosition) {
    console.log(to, from, savedPosition);
    if (savedPosition){
        return savedPosition;
    }
    return {left: 0, top: 0};
  }
});


---
## Section#13: 183. Introducing Navigation Guards
--- 

* Useful for authentication and alerting the user when moving from one page to another.

-- main.js
router.beforeEach(function(to, from, next) {
  next(false); //This will stop the user from going to another page
});


* This function receives 3 arguments from Vue router
    1. to: router object we are going from
    2. from: router object to which we are going
    3. next: true, false or pass a url or route object



---
## Section#13: 184. Diving Deeper Into Navigation Guards
--- 

* We can also navigation guard on our individual routes

-- main.js
{ path: '/users', components: { default: UsersList, footer: UsersFooter } , 
  beforeEnter(to, from, next) {
      console.log('users beforeEnter');
      next();
  }},

* We can also define this guard inside the component using beforeRouteEnter()
-- UsersList.vue
<script>
import UserItem from './UserItem.vue';

export default {
  components: {
    UserItem,
  },
  inject: ['users'],
  methods: {
    confirmInput() {
      this.$router.push('/teams');
    }
  },
  beforeRouteEnter(to, from, next) {
    console.log('UsersList component beforeRouteEnter()');
    next();
  }
};
</script>

* If they are defined in all the places, the order of executing would be

  1. Global beforeEach()
  2. Global component level beforeEnter()
  3. Component level beforeRouterEnter()

* There are two other guards related to loading the component
  1. beforeRouteUpdate(): this is executed whenever the component is reused. This can be alternative to watching teamId in TeamMembers.vue

---
## Section#13: 185. The Global "afterEach" Guard
--- 

* After each will not get the third argument

router.afterEach(function(to, from) {
  // sending analytics data 
});

---
## Section#13: 186. Beyond Entering: Route Leave Guards
--- 

* Guard we can call before leaving a page using beforeRouteLeave(to, from, next) inside component

  beforeRouteLeave(to, from, next) 

  Ofcourse, we could do it with unmounted() method on the component, but this wont allow us to cancel the navigation if
  we dont want the user to leave the page.

---
## Section#13: 187. Utilizing Route Metadata
--- 

* We could pass meta data long with our routes and access them via $route, we can also access them in navigation guards 
    in to and from objects.

-- main.js
{
  name: 'teams',
  path: '/teams',
  meta: { needsAuth: true},
  components: { default: TeamsList, footer: TeamsFooter },
  children: [
    {
      name: 'team-members',
      path: ':teamId',
      component: TeamMembers,
      props: true,
    },
  ],
},

router.beforeEach(function (to, from, next) {
  console.log('Global router.beforeEach()');
  if (to.meta.needsAuth) {
    console.log('Needs auth!');
  }
  next();
});

---
## Section#13: 188. Organizing Route Files
--- 

* Looks good if we could move the router files to a new folder called /pages for better organizing.
    This basically helps us differentiate what componets are rendered as pages, and what components are embeded
    into other components.

* Also main.js is getting bigger, we can move our router related code to a new file router.js and import it in main.js


---
## Section#13: 189. Summary
--- 

-- main.js
import { createApp } from 'vue';

import App from './App.vue';
import router from './router.js';

const app = createApp(App);

app.use(router);

app.mount('#app');


-- router.js
import { createRouter, createWebHistory } from 'vue-router';
import TeamsList from './pages/TeamsList.vue';
import UsersList from './pages/UsersList.vue';
import TeamMembers from './components/teams/TeamMembers.vue';
import NotFound from './pages/NotFound.vue';
import TeamsFooter from './pages/TeamsFooter.vue';
import UsersFooter from './pages/UsersFooter.vue';

const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: '/', redirect: '/teams' },
    // {path: '/teams', component: TeamsList, alias: '/'},
    {
      name: 'teams',
      path: '/teams',
      meta: { needsAuth: true },
      components: { default: TeamsList, footer: TeamsFooter },
      children: [
        {
          name: 'team-members',
          path: ':teamId',
          component: TeamMembers,
          props: true,
        },
      ],
    },
    {
      path: '/users',
      components: { default: UsersList, footer: UsersFooter },
      beforeEnter(to, from, next) {
        console.log('users beforeEnter');
        next();
      },
    },
    { path: '/:notFound(.*)', component: NotFound },
  ],
  // linkActiveClass: 'active',
  // linkExactActiveClass: 'inactive',
  scrollBehavior(to, from, savedPosition) {
    // console.log(to, from, savedPosition);
    if (savedPosition) {
      return savedPosition;
    }
    return { left: 0, top: 0 };
  },
});

router.afterEach(function (to, from) {
  // sending analytics data
  console.log('Global router.afterEach(to, from)', to, from);
});

router.beforeEach(function (to, from, next) {
  console.log('Global router.beforeEach()');
  if (to.meta.needsAuth) {
    console.log('Needs auth!');
  }
  next();
});

export default router;


-- components/nav/TheNavigation.vue
<template>
  <header>
    <nav>
      <ul>
        <li>
          <router-link to="/teams">Teams</router-link>
        </li>
        <li>
          <router-link to="/users">Users</router-link>
        </li>
      </ul>
    </nav>
  </header>
</template>


<style scoped>
header {
  width: 100%;
  height: 5rem;
  background-color: #11005c;
}

nav {
  height: 100%;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

li {
  margin: 0 2rem;
}

a {
  text-decoration:none;
  background: transparent;
  border: 1px solid transparent;
  cursor: pointer;
  color: white;
  padding: 0.5rem 1.5rem;
  display: inline-block;
}

a:hover,
a:active,
a.router-link-active {
  color: #f1a80a;
  border-color: #f1a80a;
  background-color: #1a037e;
}
</style>

-- pages/TeamsList.vue
<template>
  <router-view></router-view>
  <ul>
    <teams-item
      v-for="team in teams"
      :key="team.id"
      :id="team.id"
      :name="team.name"
      :member-count="team.members.length"
    ></teams-item>
  </ul>
</template>

<script>
import TeamsItem from '../components/teams/TeamsItem.vue';

export default {
  components: {
    TeamsItem,
  },
  inject: ['teams'],
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 2rem auto;
  max-width: 40rem;
  padding: 0;
}
</style>

-- components/teams/TeamsMembers.vue
<template>
  <section>
    <h2>{{ teamName }}</h2>
    <ul>
      <user-item
        v-for="member in members"
        :key="member.id"
        :id="member.id"
        :name="member.fullName"
        :role="member.role"
      ></user-item>
    </ul>
    <router-link to="/teams/t2">Go to Team2</router-link>
  </section>
</template>

<script>
import UserItem from '../users/UserItem.vue';

export default {
  props: ['teamId'],
  inject: ['users', 'teams'],
  components: {
    UserItem
  },
  data() {
    return {
      teamName: '',
      members: []
    };
  },
  methods: {
    loadTeamMembers(teamId) {
      const selectedTeam = this.teams.find(team => team.id === teamId);
      const members = selectedTeam.members;
      const selectedMembers = [];
      for (const member of members) {
        const selectedUser = this.users.find(user => user.id == member);
        selectedMembers.push(selectedUser);
      }
      this.members = selectedMembers;
      this.teamName= selectedTeam.name;
    }
  },
  created() {
    this.loadTeamMembers(this.teamId);
    console.log(this.$route.query);
  },
  beforeRouteUpdate(to, from, next) {
    console.log('TeamMembers Cmp beforeRouteUpdate');
    // this.loadTeamMembers(to.params.teamId);
    next();
  },
  watch: {
    teamId(newId) {
      this.loadTeamMembers(newId);
    }
  }
};
</script>

<style scoped>
section {
  margin: 2rem auto;
  max-width: 40rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 1rem;
  border-radius: 12px;
}

h2 {
  margin: 0.5rem 0;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>


-- components/teams/TeamsItem.vue
<template>
  <li>
    <h3>{{ name }}</h3>
    <div class="team-members">{{ memberCount }} Members</div>
    <router-link :to="teamMembersLink">View Members</router-link>
  </li>
</template>

<script>
export default {
  props: ['id', 'name', 'memberCount'],
  computed: {
    teamMembersLink() {
      // return '/teams/' + this.id;
      return {
        name: 'team-members',
        params: { teamId: this.id },
        query: { sort: 'asc' },
      };
    },
  },
};
</script>

<style scoped>
li {
  margin: 1rem 0;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  border-radius: 12px;
  padding: 1rem;
}

li h3 {
  margin: 0.5rem 0;
  font-size: 1.25rem;
}

li .team-members {
  margin: 0.5rem 0;
}

a {
  text-decoration: none;
  color: white;
  display: inline-block;
  padding: 0.5rem 1.5rem;
  background-color: #11005c;
}

a:hover,
a:active {
  background-color: #220a8d;
}
</style>

-- pages/UsersList.vue
<template>
  <button @click="confirmInput">Confirm</button>
  <ul>
    <user-item v-for="user in users" :key="user.id" :name="user.fullName" :role="user.role"></user-item>
  </ul>
</template>

<script>
import UserItem from '../components/users/UserItem.vue';

export default {
  components: {
    UserItem,
  },
  inject: ['users'],
  methods: {
    confirmInput() {
      this.$router.push('/teams');
    }
  },
  beforeRouteEnter(to, from, next) {
    console.log('UsersList component beforeRouteEnter()');
    next();
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 2rem auto;
  max-width: 20rem;
  padding: 0;
}
</style>

-- components/users/UserItem.vue
<template>
  <li>
    <h3>{{ name }}</h3>
    <div class="role" :class="roleClass">{{ role }}</div>
  </li>
</template>

<script>
export default {
  props: ['name', 'role'],
  computed: {
    roleClass() {
      if (this.role === 'Engineer') {
        return 'role--engineer';
      }
      if (this.role === 'Consultant') {
        return 'role--consultant';
      }
      return null;
    },
  },
};
</script>

<style scoped>
li {
  margin: 1rem 0;
  border: 1px solid #ccc;
  padding: 1rem;
}

h3 {
  margin: 0.5rem 0;
}

.role {
  border-radius: 40px;
  background-color: #ccc;
  color: #252525;
  padding: 0.25rem 1rem;
  display: inline-block;
}

.role--engineer {
  background-color: blue;
  color: white;
}

.role--consultant {
  background-color: #af003a;
  color: white;
}
</style>

-- pages/TeamsFooter.vue
<template>
    <h2>Teams Footer</h2>
</template>

-- pages/UsersFooter.vue
<template>
    <h2>Users Footer</h2>
</template>




---
# Section#14: Animations & Transitions
---



---
## Section#14: 192. Animation Basics & CSS Transitions
---

* Basically the animation is done via CSS here. We will animate the element by adding the CSS class dynamically.
    Here, we will move the div element with ".block" class to left. 
* Here we make use of two CSS properties called "transform" and "transition"

 -- App.vue
 <template>
  <div class="container">
    <div class="block" :class="{animate: animatedBlock}"></div>
    <button @click="animateBlock">Animate</button>
  </div>
  <base-modal @close="hideDialog" v-if="dialogIsVisible">
    <p>This is a test dialog!</p>
    <button @click="hideDialog">Close it!</button>
  </base-modal>
  <div class="container">
    <button @click="showDialog">Show Dialog</button>
  </div>
</template>  

<script>
export default {
  data() {
    return { 
      animatedBlock: false,
      dialogIsVisible: false 
    };
  },
  methods: {
    animateBlock() {
      this.animatedBlock = !this.animatedBlock;
    },
    showDialog() {
      this.dialogIsVisible = true;
    },
    hideDialog() {
      this.dialogIsVisible = false;
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
button {
  font: inherit;
  padding: 0.5rem 2rem;
  border: 1px solid #810032;
  border-radius: 30px;
  background-color: #810032;
  color: white;
  cursor: pointer;
}
button:hover,
button:active {
  background-color: #a80b48;
  border-color: #a80b48;
}
.block {
  width: 8rem;
  height: 8rem;
  background-color: #290033;
  margin-bottom: 2rem;
  transition: transform 0.3s ease-out;
}
.container {
  max-width: 40rem;
  margin: 2rem auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 2rem;
  border: 2px solid #ccc;
  border-radius: 12px;
}
.animate {
  transform: translateX(-150px);
}
</style>


---
## Section#14: 193. Understanding CSS Animations
---

* We can also setup a more detailed animation by setting up our own animation and apply it in our CSS.
  - Here "scale()" property basically changes the size of the element, 1 is original size. 1.1 means 10% bigger.
  - Here "slide-fade" is a user given name, we can name it to anything
  - "forwards" tells the animation to stay at the final animation, otherwise it snaps back to its original position

.block {
  width: 8rem;
  height: 8rem;
  background-color: #290033;
  margin-bottom: 2rem;
  /* transition: transform 0.3s ease-out; */
}

.animate {
  /* transform: translateX(-150px); */
  animation: slide-fade 0.3s ease-out forwards;
}

@keyframes slide-fade {
  0% {
    transform: translateX(0) scale(1);
  }

  70% {
    transform: translateX(-120px) scale(1.1);
  }

  100% {
    transform: translateX(-150px) scale(1);
  }
}


---
## Section#14: 194. Why is "Just CSS" Not Enough?
---

* If we are using just two styles like only "0% {}" and "100% {}", then we can just use shorthands "from" and "to"
* Lets add animation to dialog, this gives a nice animation of dialog

-- BaseModel.vue
dialog {
  position: fixed;
  top: 30vh;
  width: 30rem;
  left: calc(50% - 15rem);
  margin: 0;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  border-radius: 12px;
  padding: 1rem;
  background-color: white;
  z-index: 100;
  border: none;
  animation: modal 0.3s ease-out forwards;
}

@keyframes modal {
  from {
    opacity: 0;
    transform: translateY(-50px) scale(0.9);
  }
  
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}


* BUT THE PROBLEM STARTS, IF WE ALSO WANT TO ANIMATE THE REMOVAL OF THE ELEMENT
  - Currently when we close the dialog, it is removed from the DOM. That means there is no way
      to animate it with CSS. So Vue helps us delaying the disappearance of the elements until a 
      certain animation has finished.


---
## Section#14: 195. Playing CSS Animations with Vue's Help
---

"<transition></transition>"

* There is a built-in component called "<transition>" which we can wrap around the elements 
  where you wanna control the appearance and removal animation with Vue.

* "transition" must have only one direct child

* What does "transition" do with that wrapped element now ?
  * When the component (eg: modal) is mounted, "transition" adds couple of CSS utility classes to the element.
    - "v-enter-from" class
    - "v-enter-active" class
    - "v-enter-to" class

  * It adds "v-enter-from" first, "v-enter-active" at the same time. Then it removes "v-enter-from", 
      but "v-enter-active" stays on that element. And "v-enter-to" is added, right when the animation
      finishes.

  * When the component is unmounted using v-if, Vue adds
    - "v-leave-from" class
    - "v-leave-active" class
    - "v-leave-to" class    

* Vue will analyze the CSS code we write from these classes. It will read the duration of the transitions
    and animations you define in those classes, and then, and thats the key thing. It will only really
    remove the element from the DOM once that duration is over. And thats the key difference to the non-Vue
    hebavior. There elements are instantly added and removed with the transition component, and these 
    special CSS classes Vue will add and look for, Vue is able to tell that an animation has taken place
    and how long that animation takes, and it then only removes the element once that animation is done.

---
## Section#14: 196. Using the Transition Component
---

