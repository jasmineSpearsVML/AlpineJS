# Alpine JS Course Part 2

## Contents
1. [Function Component Calls](#function-component-calls)

### Function Component Calls

Recall you establish a scope by adding `x-data` to an element. You don't need to add any object inside of it if a function being called is within the scope of `x-data`, like so:

```html
<section>
    <button x-data @click="alert('Alert!')" class="bg-gray-200 p-1 rounded hover:bg-gray-300 border border-1 border-gray-500">
        Trigger Alert
    </button>
</section>
```

<video width='600' height='200' controls>
    <source src="../vid/function-call1.mov" type="video/mp4">
</video>

However, if your function being called is defined elsewhere, you'll want to create components. Since `x-data` expects an object, you can create your own component function that returns some object, and that object can have a function as a property like normal. When you execute the component function, pass it in to `x-data`. Then whatever `x-on` or other behavior you connect to your element will pull in the returned value.

```html
<script>
    const myComponent = () => {
        return {
            hello() {
                alert("Hello?");
            }
        }
    }
</script>
<section>
    <button x-data="myComponent()" @click="hello()" class="bg-gray-200 p-1 rounded hover:bg-gray-300 border border-1 border-gray-500">
        Say Hello!
    </button>
</section>
```

<video width='600' height='200' controls>
    <source src="../vid/function-call2.mov" type="video/mp4">
</video>

You can pass in values into your returned component object with `x-model`.

If your method within the returned object does not have parameters, you don't necessarily have to add the parentheses.

```html
<script>
    myComponent = function () {
        return {
            myName: '',
            hello() {
                alert("Hello?");
            },
            sayMyName() {
                /* 
                    note: ES6 function shape won't work here because `this` would refer to the global Window object, not the returned object.
                */
                if (this.myName) {
                    alert(`Are you ${this.myName}?`);
                    this.myName = '';
                }
            }
        }
    }
</script>
<section x-data="myComponent()">
    <button @click="sayMyName()" class="bg-gray-200 p-1 rounded hover:bg-gray-300 border border-1 border-gray-500">
        SAY MY NAME !!!
    </button>
    <form @submit.prevent="sayMyName" class="inline">
        <input type="text" x-model="myName" placeholder="Enter Name" class="ml-2"> 
    </form>
</section>
```

<video width='600' height='200' controls>
    <source src="../vid/function-call3-this.mov" type="video/mp4">
</video>

Like functions in general, you can add default parameters. You can also pass parameters in directly to a child function of the returned object. Notice how the first section passes in an argument to the `myComponent`, but the second section passes in an argument directly into the child function executed when clicking.

```html
<script>
    myComponent = function (startName = '') {
        return {
            myName: startName,
            hello() {
                alert(`Hello ${this.myName}`);
            },
            sayMyName() {
                if (this.myName) {
                    alert(`Are you ${this.myName}?`);
                    this.myName = '';
                }
            },
            hello_param(hello_name = '') {
                alert(`Hi ${hello_name}`);
            }
        }
    }
</script>
<section>
    <button x-data="myComponent('Yuri')" 
    @click="hello()" 
    class="bg-gray-200 p-1 rounded hover:bg-gray-300 border border-1 border-gray-500">
        Say Hello!
    </button>
</section>
<!-- pass param directly into method -->
<section>
    <button x-data="myComponent()"
    @click="hello_param('Barbie')" 
    class="bg-gray-200 p-1 rounded hover:bg-gray-300 border border-1 border-gray-500">
        Say Hello! (with params)
    </button>
</section>
```

### `X-transition`s with modifiers on `x-show`

https://alpinejs.dev/directives/transition
https://tailwindcss.com/docs/transition-property

The `x-transition` property can be modified with various properties that can affect an element's appearance and how long it takes to transition. I just added related documentation links as I think most of these examples in the code are pretty self-explanatory at this point, especially if you run them locally. This course makes use of Tailwind CSS to modify CSS properties. You can also use other custom and user-defined classes to do what you want. Otherwise, you would have to use pure CSS.

### `X-cloak`

https://alpinejs.dev/directives/cloak

`X-cloak` is used to disable flashing content on the page before Alpine can load. Here, `x-cloak` prevents the submenu from briefly flashing when the page loads. To demonstrate this, open your DevTools and disable the cache under the Network tab. Then refresh to see.

```html
<style>
    [x-cloak] { 
        display: none !important;
    }
</style>
<section x-data="{isOpen : false}" class="relative mt-8">
    <div
    @click="isOpen = !isOpen"
    @click.away="isOpen = false"
    id="button" class="bg-blue-200 hover:bg-blue-300 inline-block px-2 py-1 cursor-pointer">Open Menu</div>
    <!-- Submenu -->
    <div
    x-cloak
    x-show="isOpen"
    x-transition:enter="transition duration-4000"
    x-transition:enter-start="opacity-0"
    x-transition:enter-end="opacity-100"
    x-transition:leave="transition duration-1000"
    x-transition:leave-start="opacity-100"
    x-transition:leave-end="opacity-0"
    class="mt-2 z-10 absolute" id="submenu">
        <a target="_blank" class="px-2 py-1 inline-block w-32 cursor-pointer bg-blue-200 hover:bg-blue-300" href="https://www.google.com">Google</a><br>
        <a target="_blank" class="px-2 py-1 inline-block w-32 cursor-pointer bg-blue-200 hover:bg-blue-300" href="https://www.amazon.com">Amazon</a><br>
    </div>
</section>
```

### `X-for`

https://alpinejs.dev/directives/for

`X-for` is used for looping. You can access an item's index akin to the [`forEach` loop in JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach), that in which you can access the iterated element, its index, and the array it comes from as arguments. This is useful if you need to keep a collection in order.

When looping, the required `template` element must only have one root element, similar to React components. Otherwise it will not render the content correctly.

If the array I want to loop over is created within `x-data`'s object scope, then the array I want to loop over can be accessed directly into the template.

```html
<section x-data="{
    names: [
        'Kristi',
        'Claudia',
        'Mary Anne'
    ]
}">
    <template x-for="name in names">
        <div>
            <span x-text="name"></span><br>
        </div>
    </template>
</section>
```

I can access object properties to populate content as well.

```html
<section x-data="{
    persons: [
        {
            name: 'Dawn',
            age: 13,
        },
        {
            name: 'Jessi',
            age: 11,
        },
        {
            name: 'Mallory',
            age: 11,
        }
    ]
}">
    <template x-for="person in persons">
        <div>
            Name: <span x-text="person.name"></span>
            Age: <span x-text="person.age"></span><br>
        </div>
    </template>
</section>
```

If the array is declared outside of the scope, then it can be passed in as a property value in the `x-data` object.

```html
<script>
    let personsOutsourced = [
        {
            name: 'Dawn',
            age: 13,
        },
        {
            name: 'Jessi',
            age: 11,
        },
        {
            name: 'Mallory',
            age: 11,
        }
    ];
</script>
<section x-data="{ persons: personsOutsourced }">
    <template x-for="person in persons">
        <div>
            Name: <span x-text="person.name"></span>
            Age: <span x-text="person.age"></span><br>
        </div>
    </template>
</section>
```

Similarly, this can be passed in from a file.

```html
<head>
    <script src="js/persons.js">
        // content of persons.js
        let personsFromFile = [
            {
                name: 'Stacey',
                age: 14,
            },
            {
                name: 'Abby',
                age: 13,
            },
            {
                name: 'Shannon',
                age: 13,
            }
        ];
    </script>
</head>
...
<section x-data="{ persons: personsFromFile }">
    <template x-for="person in persons">
        <div>
            Name: <span x-text="person.name"></span>
            Age: <span x-text="person.age"></span><br>
        </div>
    </template>
</section>
```
