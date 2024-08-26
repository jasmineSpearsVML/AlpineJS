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

