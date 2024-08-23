# Alpine JS Course Part 1

Source: https://www.udemy.com/course/alpinejs

## Contents
1. [Object Scope](#object-scope)
2. [X-Data and X-Text](#x-data-and-x-text)
3. [X-Bind](#x-bind)
4. [X-Model](#x-model)
5. [X-Show and X-If](#x-show-and-x-if)

### Object Scope

Like other situations, parent elements encapsulate defined attributes and descendants inherit them. Sibling elements that have a defined value for an `x-data` attribute do not share scopes, so they could be defined with identical ones.

### x-data and x-text

If a parent element contains an `x-data` attribute and also has an `x-text` attribute defined that carries that `x-data` attribute, then child elements will not be able to display the value of `x-data` using the `x-text` attribute. It will break and say in the console that the value for the `span`'s `x-text` is not defined.

It is valid for the parent element to have both attributes, but a child element also containing an `x-text` attribute will error.

This works:

```html
<div x-data="{div1obj: 'div1 object'}" class="bg-blue-200 w-144 relative p-3 mb-6 rounded shadow">
    <b>This is Div 1</b><br><br>
    <span x-text="div1obj"></span>
    <br>
    <div x-data="{nested: 'hello'}" class="bg-white absolute top-2 right-2">
        bonjour
        <span x-text="nested"></span>
        <span x-text="div1obj"></span>
    </div>
</div>
```

![result of x-data div](../img/result1.png)

This does not, as the first nested `div` has an `x-text` defined and its children elements also have an `x-text` defined:

```html
<div x-data="{div1obj: 'div1 object'}" class="bg-blue-200 w-144 relative p-3 mb-6 rounded shadow">
    <b>This is Div 1</b><br><br>
    <span x-text="div1obj"></span>
    <br>
    <div x-data="{nested: 'hello'}" x-text="nested" class="bg-white absolute top-2 right-2">
        bonjour
        <span x-text="nested"></span>
        <span x-text="div1obj"></span>
    </div>
</div>
```

![errored result of x-data div](../img/error1.png)

You can also override `x-data` attributes with a child element:

```html
<div x-data="{div1obj: 'div1 object'}" class="bg-blue-200 w-144 relative p-3 mb-6 rounded shadow">
    <b>This is Div 1</b><br><br>
    <span x-text="div1obj"></span>
    <br>
    <div x-data="{nested: 'hello', div1obj: 'I am overridden'}" 
        class="bg-white absolute top-2 right-2">
        bonjour
        <span x-text="nested"></span>
        <span x-text="div1obj"></span>
    </div>
</div>
```

![override x-data](../img/override1.png)

### x-bind

X-bind doesn't need to have `x-bind` on an element, such as `x-bind:value="xyz"`. It can just be `:value="xyz"`.

You can also manipulate attributes by using binding, such as classes or disabling.

As you can see, adding a key in the second `input`'s `x-data` as `isDisabled`, setting to `true`, and then adding an `x-bind` to the `input`'s `disabled` attribute set to `isDisabled` can dynamically update the value set.

```html
<input x-data="{ myInputVal: 'some value' }"
    class="border border-1 p-1 rounded shadow" 
    :value="myInputVal" placeholder="some placeholder" type="text">

<input x-data="{
    myInputVal: 'some other value',
    isDisabled: true
    }" 
    :disabled="isDisabled"
    class="border border-1 p-1 rounded shadow" 
    :value="myInputVal" placeholder="some placeholder" type="text">
```

![x-bind attributes](../img/x-bind1.png)

You can add or append styles and classes to an element using `x-bind`.

```html
<span x-data="{myStyle: 'font-weight:700'}" :style="myStyle">Some text</span>
```

![x-bind styling](../img/x-bind2.png)

If you want to preserve an existing style or class and then append to it, you can use concatenation. If a `class` exists separately from `:class`, then they will be combined, with the `class` values placed first.

```html
 <div x-data="{myClass: 'bg-green-500'}" 
    :class="'rounded-xl ' + myClass"
    style="height: 4rem; width: 4rem; border: 1px solid black;">
</div>
```

Be careful; for things like styles and classes, enclose the pre-existing values in a single quote and then separate the adjacent value with a space before concatenating the data you are binding to.

![x-bind classes](../img/x-bind3.png)

### x-model

In x-model, the value of elements and objects influence each other - this is two way binding.

In this first example, I set up my initial value with `x-data` on the encasing `section` element with a key of `myInput`, which here is a blank value. I make the `input` carry the `x-model` of this value, which will be affected by whatever I add into the `input` field.

I then add an `x-text` attribute onto the adjacent `span` element that will dynamically update its value based on what the value of `x-model` is.

```html
<section x-data="{ myInput: '' }">
    <input x-model="myInput" class="border border-1 p-1 rounded shadow" type="text">
    <span x-text="'Output: ' + myInput">Output: </span>
</section>
```

<video width='600' height='300' controls>
    <source src="../vid/x-model1.mov" type="video/mp4">
</video>


In the next example, I made two inputs be affected by each other. Similar to the previous example, I create an initial key/value pair on the encasing `section` as `myValue : 'Hello'`. I place an `x-model` on both adjacent `input`s as they will both be influencing each other's value. I then bind them both with the initial value of `myValue`. As I update one input, the other will match each other's value.

```html
<section x-data="{ myValue: 'Hello' }">
    <input x-model="myValue" :value="myValue" class="border border-1 p-1 rounded shadow" type="text">
    <input x-model="myValue" :value="myValue" class="border border-1 p-1 rounded shadow" type="text">
</section>
```
<video width='600' height='200' controls>
    <source src="../vid/x-model2.mov" type="video/mp4">
</video>


In the last example, I made a `div` and a `select` list have their background color (here determined by class) affected by whatever a selected option's value is. Since the `select` originally had a long string of classes, for readability, I moved them into another key/val pair inside of the `x-data` so that I can concatenate them to the class that influences its background color. Since I am setting the classes with a key from `x-data`, I use `x-bind` to tie them together. 

Recall that the value of `select` is whatever `option` is chosen, so that value carries into the `x-model` that is set on the `select`. That value passing into `myColor` influences the background color of both the `div` and the `select`.

Note that the `option` tags don't necessarily need to have a `value` attribute unless I want to override whatever may be set within the nodes themselves, hence the first `option` still carrying the `value` of gray.

```html
<section x-data="{ 
        myColor: 'gray', 
        selectClasses: ' h-full p-2 rounded ml-3 border border-gray-900 text-white focus:outline-none'
    }" 
    class="flex">
    <div :class="'bg-' + myColor + '-600'" style="height: 5rem; width: 5rem; border: 1px solid #555;"></div>
    <select x-model="myColor" :class="'bg-' + myColor + '-600' + selectClasses">
        <option value="gray">Select color</option>
        <option>green</option>
        <option>blue</option>
        <option>red</option>
    </select>
</section>
```

<video width='600' height='300' controls>
    <source src="../vid/x-model3.mov" type="video/mp4">
</video>

### x-show and x-if

You can toggle values using event handlers. Handlers are preceded with an `@` sign. You can then use negation to toggle a `true` or `false` value when clicking, for example. 

You can also use ternary operations to toggle values of things like `x-text` and binding classes based on the value of the data's key, for example.

As the names imply, `x-show` uses a boolean value to determine if an element is visible by setting the `display` style attribute. `X-if` uses a boolean value to include the element in the DOM if truthy, and requires a `template` surrounding the element in question.

In the first example, the text in the `span` displays if the value of `showText` is true, and hides if not. It is not gone from the DOM, it just has a style attribute of `display: none;`. `showText` toggles based on the user clicking the button. This value also affects the style `:class` and the content (`x-text`) of the button by using ternary operators.

```html
<section x-data="{ 
    showText: false,
    buttonClass: ' transition p-1 rounded shadow focus:outline-none'
    }">
    <button @click="showText = !showText" 
    x-text="showText ? 'Hide' : 'Show'" 
    :class="showText ? 'bg-red-300 hover:bg-red-400' + buttonClass : 'bg-green-300 hover:bg-green-400' + buttonClass">Show</button>
    <span x-show="showText" class="bg-gray-200 ml-3 p-2">I am supposed to be seen only when <b>show == true</b>.</span>
</section>
```

<video width='500' height='200' controls>
    <source src="../vid/x-show.mov" type="video/mp4">
</video>

In the second example, the idea is essentially the same, except for this a `template` tag is needed to surround the element that I want to add or remove from the DOM. 

```html
<section x-data="{ 
    showText: false,
    buttonClass: ' transition p-1 rounded shadow focus:outline-none'
    }">
    <button @click="showText = !showText" 
    x-text="showText ? 'Hide' : 'Show'" 
    :class="showText ? 'bg-red-300 hover:bg-red-400' + buttonClass : 'bg-green-300 hover:bg-green-400' + buttonClass">Show</button>
    <template x-if="showText">
        <span class="bg-gray-200 ml-3 p-2">Hi! When I am gone, you won't find me in the DOM! ;-)</span>
    </template>
</section>
```

<video width='500' height='300' controls>
    <source src="../vid/x-if.mov" type="video/mp4">
</video>