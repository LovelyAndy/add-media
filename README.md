# add-media
This is a self-contained parent + child component combination. Not meant to upload anything or deal with any databases on it's own.


**Child component (UploadMediaFiles)** 

Renders an invisible `<input type="file" />` and has a slot for that input for which you can pass any type of element to cover that input.


The base component has two props:

`multiple: {type: Boolean}`
`accept: {type: String}`

When element is clicked it emits an event `$emit('selected', files)`


**Parent component (SelectAndPreviewFiles)**

- User selects an image/video file(s) through the slotted `upload button`

- The select files are instantly viewable above the `upload button`

- A `<button>delete</button>` below each file that removes the file on click

**Note**
**SelectAndPreviewFiles** does not upload anything. Only emitting an `'input'` event with an array of files 

The **SelectAndPreviewFiles** needs to be compatible with v-model, so it receieves a `value` and emits an `'input'` event
`Value`should be an array of objects `value: {type: Array}`

The `value` is also the payload for the emitted `'input'` event 

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```
