# Collection and Specialty Views

## Learning Objectives
* Define the role of collection views in Backbone.
* Define the purpose of the el property on a Backbone View.
* Write a collection view that renders model views.
* Write a collection view that listens to collection events.
* Define the role of a specialty view in Backbone.
* Create a specialty view.

## Quick Recap

This morning Adam went over three types of views we will encounter in Backbone.
  1. **Item views.** Render a single model instance and respond to its changes.
  2. **Collection views.** Render and manage a collection.
  3. **Specialty views.** Views for log-in forms, create/edit forms, etc.

For this portion of the class, we will go over the latter two.

## Collection View (5)

So we have a view for each of Grumbles. What can it do?
* Generate a DOM representation of each Grumble.
  * Defined by a Handlebars template: `grumbleTemplate`.
  * Attached to the DOM using a `render` method.
* Acts as a controller. Manages CRUD interactions with model.
  * `updateGrumble`, `deleteGrumble`.
* Generates DOM elements required for CRUD interactions.
  * Defined by a Handlebars template: `grumbleFormTemplate`.
  * A mini "specialty view" of sorts.

Next: a **collection view** that does the same thing for multiple models in different scenarios.
  * When we first load Grumblr, all of our Grumbles should be rendered.
  * When we create a new Grumble, it should be appended to our existing list of Grumbles.

## Let's Get Started (5)

If you need a starting point for the remainder of this lesson, fork and clone from [this branch](https://github.com/ga-dc/grumblr_backbone/tree/views_part_1_solution).

Let's begin as we did earlier in class and create our collection view in `/js/backbone/views/grumblesList.js`...
* What is the syntax we have used so far to create Backbone models and views?
* What function do all of our Backbone objects need?

```js
// /js/backbone/views/grumblesList.js

App.Views.GrumblesList = Backbone.View.extend({

  initialize: function(){
    console.log( "Grumbles view initialized!" );
  }

});
```

## el (5)

Next, we're going to bind our collection view to a DOM element.
* What's that mean?

In Backbone, we can define an `el` property that indicates a DOM element to which our collection view, once rendered, will automatically append to.  
* So if we set `el: "#grumbles"`, our collection view will automatically append to `<div id="grumbles"></div>` upon rendering.

```js
App.Views.GrumblesList = Backbone.View.extend({
  el: "#grumbles",

  initialize: function(){
    console.log( "Grumbles view initialized!" );
  }
});
```

Essentially, we are "coupling" our view with the DOM.
* [Some people don't like this](http://coenraets.org/blog/2012/01/backbone-js-lessons-learned-and-improved-sample-app/).
* Why do you think that is?

**NOTE** This **does not** replace th `$el` property we have been using since learning OOJS.
* We will continue to use `$el` to generate a DOM representation of our collection view.

## renderOne (10)

Our collection view is a collection of model views (i.e., `GrumbleView`).
* We're going to use this view to generate multiple model views.
* In order for this to happen, we need to tell it how to generate a single model view. Let's do this using a `renderOne` method.

Why do we need a `renderOne` method?
* Wouldn't a method that renders an entire collection do just fine?

### Exercise: Create a `renderOne` method.

1. Instantiate a function that takes a model as an argument.
2. Create a view for the passed-in model.
3. Append that model view to the collection view.

```js
App.Views.GrumblesList = Backbone.View.extend({
  el: "#grumbles",

  initialize: function(){
    console.log( "Grumbles view initialized!" );
  },

  renderOne: function( grumble ){
    var view = new App.Views.Grumble({ model: grumble });
    this.$el.prepend( view.$el );
  }
});
```

## renderAll (10)

Now our collection view's `$el` property contains all of our model views. The next step is to append that `$el` to the DOM.
* We don't need to deal with any additional Handlebars templates here since that's taken care of inside each of our model views.

### Exercise: Create a `renderAll` method.

1. Clear out the collection view's current $el property.
2. Iterate through our collection (i.e., our collection's models) and render its individual model views.

```js
App.Views.GrumblesList = Backbone.View.extend({
  el: "#grumbles",

  initialize: function(){
    console.log( "Grumbles view initialized!" );
  },

  renderOne: function( grumble ){
    var view = new App.Views.Grumble({ model: grumble });
    this.$el.prepend( view.$el );
  },

  renderAll: function(){
    this.$el.empty();
    this.collection.each( this.renderOne.bind(this) );
  }
});
```

## Event Listeners (5)

Like our model view, our collection view also needs event listeners so that it re-renders upon any change.

### Exercise: Create event listeners for our collection view.

1. Create a listener that re-renders our entire view upon reset.
2. Create a listener that renders a new model view when one is added to our collection.

```js
App.Views.GrumblesList = Backbone.View.extend({
  el: "#grumbles",

  initialize: function(){
    this.listenTo(this.collection, 'reset', this.renderAll);
    this.listenTo(this.collection, 'add', this.renderOne);
  },

  renderOne: function( grumble ){
    var view = new App.Views.Grumble
  },

  renderAll: function(){
    this.$el.empty();
    this.collection.each( this.renderOne.bind(this) );
  }
});
```

## Update app.js (5)

### Exercise: Update `app.js` so our application instantiates and renders a collection view.

1. Instantiate a collection
2. Instantiate a collection view and pass in a collection.

```js
// app.js

App = {
  Models: {},
  Views: {
    grumbleViews: []
  },
  Collections: {},
  Routers: {}
};

$(document).ready(function() {
  var grumbles = new App.Collections.GrumblesList();
  var grumblesList = new App.Views.GrumblesList({ collection: grumbles });
  grumbles.fetch({ reset: true })
});
```

That's it...for now.
* As we continue building on our Backbone application, we'll revisit our collection view and add on some additional functionality.
* Wait, we don't need events here? Why?

## Break (10)

## Specialty Views (5)

So we have views for our Grumbles -- model and collection. What about one for our new Grumble form?

**NOTE: Will create own version of this diagram.**
* Will task students with creating the equivalent of `LoginView` below.

![view diagram](/img/view-diagram.png)

## Exercise: Specialty View for Creating Grumbles

### Part 1: View and Template (30)

Steps
1. Start by adding the following HTML to the top of your HTML `<body>`.

  ```html
  <div id="createGrumble">
    <button class="new">New Grumble</button>
    <div class="formContainer"></div>
  </div>
  ```

2. Create a `grumbleCreate.js` file in your views folder.
3. In that file, instantiate a new Backbone view called `GrumbleCreate`.
4. Couple your view with the DOM element you created in Step 1.
5. Create a handlebars template for your view.
6. Define an `initialize` method that inserts your template into the view's `$el`.
  * **Hint:** Use `.html()`.
7. Define a `createGrumble` method that creates a Grumble based on user input to your template form. This method should...
  * Create an object literal that contains your new Grumble model's data.
  * Save this object literal as a model in your collection.

### Bonus: Toggle Form

Your createGrumbleView should be hidden upon page load. If the user wants to create a new Grumble, he must click a "Show Form" button that reveals the form.

### Solution

```js
App.Views.GrumbleCreate = Backbone.View.extend({
  el: "#createGrumble",

  events: {
    'click .new':    'toggleForm',
    'click .cancel': 'toggleForm',
    'click .submit': 'createGrumble'
  },

  initialize: function(){

    this.template = Handlebars.compile($("#grumbleFormTemplate").html());

    this.$(".formContainer").html(this.template({}));
    this.$(".formContainer").hide();
  },

  toggleForm: function(){
    this.toggleButton(this.$(".new").text());
    this.toggleUrlState(this.$(".new").text());
    event.preventDefault();
    this.$(".formContainer").slideToggle();
  },

  toggleButton: function(state){
    if(state === "New Grumble"){
      this.$(".new").text("Hide Form")
    }else{
      this.$(".new").text("New Grumble")
    }
  },

  createGrumble: function(){
    event.preventDefault();
    var data = {
      title: this.$("[name='title']").val(),
      authorName: this.$("[name='authorName']").val(),
      content: this.$("[name='content']").val(),
      photoUrl: this.$("[name='photoUrl']").val()
    }
    this.collection.create(data);

    this.$el.find("input, textarea").val("");
    this.toggleForm();
  },
});
```

## Break (10)
