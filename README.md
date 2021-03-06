# Angular UI: ngClass and CSS animations

### Objectives
- Dynamically apply and remove css classes using `ng-class`
- Animate elements with CSS transitions or keyframe animations

## CSS Classes in Angular - Intro (5 mins)

As you know, Angular is a great, dynamic front-end framework. One of the things you'll do _most_ often when building a frontend is apply CSS classes to style elements on the page. Having the ability to change those styles based on _data_  can enable you to make some really dynamic interface designs. You certainly have the skills to do this with tools you've already learned, but Angular has a special directive just for this purpose – **ngClass**.

Once we've looked at how Angular helps us apply classes, we'll mix in some non-Angular-specific strategies to add subtle animation to our style with CSS. Little things like CSS animation can really boost the credibility of your design – _if_ you use it well. As a first step, you have to know how to use it at all.

Finally, you'll incorporate two external Angular modules to kick your UI up a notch.

## Starter Code - Intro (5 mins)

Now, go check out the starter code in `starter-code/app` and see what we're working with. The starter code is a basic Angular app using stuff you've mostly already seen.

Take a few minutes and browse through the code that's there and investigate how it works and what it's doing.

## Add UI-Bootstrap - Independent Practice (5 minutes)

The community-made library UI-Bootstrap refactors many Bootstrap Components that relied on jQuery to instead use Angular.  

Follow the UI-Bootstrap getting started guide to add UI-Bootstrap to your project.  Don't worry about changing the styles of existing elements in the starter code, but as you experiment with adding elements today, you can try the UI-Bootstrap versions.

The Bootstrap docs say you should add `<meta name="viewport" content="width=device-width, initial-scale=1">`.  What does the `viewport` meta tag do? 

## ngClass - Codealong (10 mins)

`ngClass` is a pretty simple directive. There are a couple of possible ways to use it, but it needs to know the name of a class and whether the element should have that class or not - it uses a value of `true` or `false` to decide if it should apply the class.

As you can imagine, `ngClass` can be applied to any HTML tag. If it can handle normal classes, it can handle `ngClass`. But keep in mind, `ngClass` is designed for adding/removing classes, so if you don't expect the classes to change, just use the normal `class="something"` attribute.

Now let's give this a shot: Like in our finished example, we want to apply a class of `error` – we've already got a style for that that will make the borders red so it's obvious something's wrong.

We need a boolean expression that will tell us if there's an "error" or not. For simplicity's sake, let's say that our only validation is whether the text of the input is four or more characters. The number here is arbitrary.

We've got this:

```html
<input type="text" id="new-winner-name" ng-model="awards.newWinner.name" ... >
```

Adding `ngClass` is as easy as:

```html
<input type="text" id="new-winner-name" ng-model="awards.newWinner.name" ng-class="{'error': awards.newWinner.name.length <= 4}" ... >
```

Let's pull that out:
```js
ng-class="{'error': awards.newWinner.name.length <= 4}"
```

`ng-class` is just an attribute we add, a directive like we've seen before. In the quotes is a javascript object, where the key is the class name and the value is an expression that evaluates to `true` or `false`.

The value associated with each key could be a plain boolean like `true`, a simple expression like `awards.newWinner.name.length <= 4` above, or even a function call. Or, you could have a function that returns the entire object inside the ng-class string. Angular just needs to be able to evaluate the ng-class string to read like:

```js
{'error': true}
```

Since ng-class translates the string to a JavaScript object, it's _easy_ to have multiple classes that get turned on/off via multiple dynamic booleans.

```js
{'error': true, 'wombat': false, 'super': true, 'so-cool': true}
```

Would equate to:

```html
<input ... class="error super so-cool" ... >
```

## ngClass - Independent Practice (10 mins)

Now, go crazy. You have 10 minutes - start experimenting with your own `ngClass` expressions:

- Add more class names and booleans as properties of your `ngClass` object string
- Try `ngClass` inside the `ngRepeat`
- Make a checkbox with a class directly on it that toggles some style on/off
- Make some changeable data in one part of the interface change the style of an element style in a separate part of the interface
- Try using an `ngClick` to toggle an arbitrary piece of data that affects multiple pieces of the interface at once

Play with it and you'll start seeing how powerful it is. Let's see what you come up with!

## Angular Form Validation - Independent Practice (10 minutes)

Angular has built-in validation for many form fields. 

Add a field to the form for a "reason" for the award, and update the list of winners to display what they won the award for.

Require the user to input a reason for the new winner. Try submitting the form with a blank reason, and see what happens. 

Give the reason field a minimum length of three and a maximum length of thirty. (Hint: look at the docs for `input[text]`.) Now what happens if you submit the form with the wrong length reason?  Disable the submit button if the form is invalid (Hint: look up `$invalid`.)

## Share out (10 mins)

<!-- 55 mins -->

## Break! (10 mins)

## CSS Transitions - Intro (5 mins)

Now that we've got that down, let's talk about our other sweet trick – animation in CSS.

CSS supports two main kinds of animation: **transitions** and **keyframe animations**. Angular has a wrapper for many of the base CSS animations called `ngAnimate`. However, today we'll look at those base CSS animations. 

They both result in styles changing over time, and both don't give you any extra styling – you can only animate using CSS properties that already exist.

**Transitions** are generally used as a reaction to a user's action - something that changes when you hover or focus or click.

**Animations** have the ability to be more complicated, can be defined more precisely, and are more flexible. They're also more code. 

It's worth noting that just like visual design, animation is an art. If you really like using animation, take some time to research the basic principles of animation for interfaces. You (and your users) will be glad you did.

> **Important note:** Not all transitions or animations work on css-generated pseudo-elements. :before and :after work in some browsers, but not all. Test or check with a reliable source to make sure something works in your target browsers instead of assuming it will.

## CSS Transition Demo (5 minutes)

Let's start with transitions - you'll see that those are pretty simple to get started with.

Let's say, for example, when we hover over our form, it rights itself and moves a little bit:

![animated form](https://cloud.githubusercontent.com/assets/25366/9594904/8ba3527c-5016-11e5-94a5-f8affb95ffba.gif)

We can inspect our form to see how it's rotated and positioned at the beginning:

```css
form {
  /* ... */

  -ms-transform: rotate(-7deg); /* IE 9 */
  -webkit-transform: rotate(-7deg); /* Chrome, Safari, Opera */
  transform: rotate(-7deg);

  position: relative;
  left:-1rem;
  top: 0rem;
}
```

Then add a `:hover` effect that modifies those rotations and positions, something like:

```css
form:hover {
  top: -0.618rem;
  left: -0.382rem;
  -ms-transform: rotate(-2deg);
  -webkit-transform: rotate(-2deg);
  transform: rotate(-2deg);
}
```

Now the fun part:

```css
form {
  /* ... */
  transition: all 300ms ease-in-out;
}
```

The `all` means that all CSS properties will be affected by this transition. The `300ms` says the transition should last that long, in milliseconds, and the `ease-in-out` is one of a few choices that we have for _easing_. Easing makes the animation look more natural. You can list out individual properties with different transition effects like this:

```
div {
  transition: background 0.2s ease,
              padding 0.8s linear;
}
```

Note also that you can put the time in seconds.

## CSS Animation Demo (5 minutes)

Now before we try it ourselves, let's see an animation. Animations are triggered, remember. By default, they are triggered when an object appears or when a class is applied to an object.

Before we can trigger a keyframe animation, we have to _define_  it with keyframes. Imagine a timeline where you're saying "At this spot, do this. At this next spot, change that. At this other spot, it should look like this."

We do that by creating a special `@keyframes` rule, and we name it whatever we like:

```css
@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  80% {
    opacity: 0.5;
  }
  100% {
    opacity: 1;
  }
}
```

Of course, nearly any properties can be changed in each frame, and nearly any percentages can be included. You could have 100 keyframes if you want, or just two.

Once you have an animation defined, here's how you'd use it:

```css
ol li {
  animation-duration: 800ms;
  animation-name: fadeIn;
}
```

Just like a function, we reference the animation with the name we invented.

**Try refreshing the page** – see how it now fades in? **Try adding a name.** See how it fades?

## Transitions & Keyframes Independent Practice (10 minutes)

Now that we've seen two simple examples, play around with animations. Add keyframes to animate different properties. Use transitions over hover on other elements, and see what CSS properties you can subtly animate.

There are a lot of CSS properties to play with and a lot of possibilities.  For instance:

- Change font weight or color when you hover over a winner
- Trying changing both opacity and color when fading in
- How could you make something fade _out_?
- Most importantly – **what simple animations could you add to help guide a user to know what to do with this page?** How could you use animation to show them how the interface works?

## Share & Discuss (10 mins)
- What's the difference between CSS transitions and animations? Where might you use one or the other?
- How do you write an `ngClass` with multiple CSS classes?
- When do you use `ngClass` vs. normal classes?

<!-- 100 mins -->

## External Modules - Intro (5 mins)

UI-Bootstrap was built as part of a larger Angular-UI project that has many modules. (You can see their list at <a href="https://angular-ui.github.io/">Angular-UI</a>.) Those are only one group of modules; many more exist. There's no real central location to find a comprehensive list of community-built angular modules, but some sites have tried - like <a href="http://ngmodules.org/" target="_blank">ngmodules.org</a>. You'll also find articles or posts online like <a href="https://codegeekz.com/18-best-angularjs-directives-for-developers/" target="_blank">18 Best AngularJS Directives for Developers</a>. As with Ruby gems and Node modules, you'll get into the habit of checking if "there's a ____ for that." Just be careful to check when an external module was last updated, whether it has good documentation, whether it's maintained, etc.

## External Directive Independent Practice (15 mins)

Try incorporating community-made Angular modules into an existing Angular project (like the Awwwards sample).  Feel free to try any you'd like.  Here are a few ideas:

  * Add the <a href="https://github.com/urish/angular-moment" target="_blank">angular-moment</a> directive to show a date and time in your app that ticks down by the second.

  * Add the `currentWeather` directive from the <a href="https://github.com/sf-wdi-22-23/modules-22/blob/master/w10-mean/d2-homework/directives.md" target="_blank">Angular Directives reading</a>.

  * Add the <a href="https://github.com/allenhwkim/angularjs-google-maps" target="_blank">ngMap</a> directive to display a map in your app centered on SF. **Hint:** <a href="http://allenhwkim.tumblr.com/post/70986888283/google-map-as-the-simplest-way" target="_blank">This blog post</a> has some very helpful examples.




