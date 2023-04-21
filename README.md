# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

## Recommended IDE Setup

- [VS Code](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

### **A NOTE-TAKING APP MADE BY VUE 3**

_[Click here to preview live](https://vue-notes-app-self.vercel.app/)_

In this application we will be able to write a note and add it to the list of notes.

#### HOW THE NOTES APP WORKS

We have a "plus" button that when pressed, opens the notes modal.To implement this functionality, we create a `showModal ref` variable to store the state of the modal which is a `boolean` value, so either `true` or `false`. By default its false so that we dont see it when the page loads.

At the plus button, we add an event listener `@click` which will set the value of the `showModal` to `true` so that we can see our modal.

```vue modal setup
<script setup>
import { ref } from "vue";
const showModal = ref(false);
</script>

<template>
  <button @click="showModal = true"></button>
</template>
```

We have bound `showModal` value to the modal, so when we click the '+' button the value is turned `true` and the _modal_ appears. An example of what is happening behind the scenes is the following button that when clicked it disappears:

```vue conditional rendering
<script setup>
//Import ref from vue
const showHeading = ref(true);
</script>
<template>
  <button
    v-bind="showHeading"
    @click="showHeading = !showHeading"
    v-if="showHeading"
  >
    If TRUE, now you see me!
  </button>
</template>
```

As you can see, we are first binding the value of `showHeading` to the
button so that the element can change according to the value of `showHeading`.
In vue, elements are rendered conditionally, meaning if a value is `true` then
it is rendered to the DOM otherwise if `falsy` it is removed from the DOM. We
then attach an `event listener` which is `@click` which triggers the change of
`showHeading` when clicked. Lastly we have a `v-if` which controlls the
condition of `showHeading` value.

Bind >> Listen for event >> Render if true.

Now back to our notes app, if `showModal` is true, then we show the modal which is an overlay.

##### **MODAL**

The modal consists of a `textarea` , 'Add Note' & 'Close' button.

The textarea is 2-way-bound to `newNote` variable which is an empty string. This means that when we type something in the textarea, the value of `newNote` changes.

The 'Add Note' button adds the newNote to the list of notes we have.It has an event listener which triggers `addNote()` function. This function 'pushes' new note to a `notes` variable that is an empty array. addNote function pushes an object with a note's properties like `id, text, date, & backgroundColor`
The text is initialized to newNote.value which is the input on the textbox.

The 'Close Button' has an event listener that turns the value of `showModal` to `false`, hence closing the modal.

In the modal we have `v-model.trim` which is to trim our input and get rid of whitespaces in the text. We can also trim in the if condition checking for the length of our text.
`if(newNote.value.trim.length)`. But the prefered way is to do it in the v-model.

We also have a warning message which appears when the user tries to add a list when the characters are below 10.
This is implemented by having a `newNotesWarning` which is a ref of empty string. We conditionally render the newNoteWarning in a paragraph. If the newNote.value.length is less than 9, return newNoteWarning with an initialized message in it.

` if (newNote.value.length < 9) 
{
    return (newNoteWarning.value = "Note needs to be more than 10 charactera");
}`

The reason we are saying `<p v-if="newNoteWarning"> {{newNoteWarning}}</p>` is coz we know newNoteWarning is an empty string by default, and an empty string is `falsy` in JS. So, if its `true` (which is not at the start), then it will be rendered in the paragraph.
