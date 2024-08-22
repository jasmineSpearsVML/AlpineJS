# Alpine JS Course

Source: https://www.udemy.com/course/alpinejs

## Contents
1. [Object Scope](#object-scope)
2. [X-Data and X-Text](#x-data-and-x-text)
3. [X-Bind](#x-bind)

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

![result of x-data div](./img/result1.png)

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

![errored result of x-data div](./img/error1.png)

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

![override x-data](./img/override1.png)

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

![x-bind attributes](./img/x-bind1.png)

You can add or append styles and classes to an element using `x-bind`.

```html
<span x-data="{myStyle: 'font-weight:700'}" :style="myStyle">Some text</span>
```

![x-bind styling](./img/x-bind2.png)

If you want to preserve an existing style or class and then append to it, you can use concatenation. If a `class` exists separately from `:class`, then they will be combined, with the `class` values placed first.

```html
 <div x-data="{myClass: 'bg-green-500'}" 
    :class="'rounded-xl ' + myClass"
    style="height: 4rem; width: 4rem; border: 1px solid black;">
</div>
```

Be careful; for things like styles and classes, enclose the pre-existing values in a single quote and then separate the last value with a space before concatenating the data you are binding to.

![x-bind classes](./img/x-bind3.png)