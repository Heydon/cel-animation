# Cel Animation
A Sass @mixin for creating traditional frame-by-frame animations using "cel" elements, especially with SVG. Think gifs, but scalable and with more control over combining animations together in one picture.

## Install

Either simply download the `_cel-animation.scss` partial from this repo' or use Bower as below:

```
bower install --save-dev cel-animation
```

## Include in your project

`@include 'cel-animation'`

## Usage

### The markup

First you need to choose a (class) name for your animation and include the "cel" elements (the independent pictures for the animation) within this named container. These can be any type of SVG or HTML elements. You should put them in the order you want them animated.

In the following code, we are using the generic "animation-name" class on an SVG `<g>` element and our cels are `<path>` elements.

```
<g class="animation-name">
	<path d="[path coords for first frame]"></path>
	<path d="[path coords for second frame]"></path>
	<path d="[path coords for third frame]"></path>
</g>

```

### Basic example

In the this basic example, we are including the only mandatory parameter, `$cels`, a list which defines the frame duration for each "cel" element in the `.animation-name` example group.

```
.animation-name {
	@include cel-animation((1 1 1));
}

```

In this animation there are three cels and each cel is visible for just one frame. It is, therefore, `0.75s` long at the default `0.25s` frame rate.

(**Note:** The hard switch between frame visibility is made possible my using the `steps(1)` `animation-timing-function`.)

### All parameters

Only the `$cels` parameter is required. The others, should you wish to use them, should be included in the order they are below.

* `$cels` The list of cels, each an integer to represent how many frames that cel should be vissible for (*list*)
* `$frame-rate` is the duration of each frame's appearance. Eg. 0.25 (the default) means 0.25 frames per second (fps) (*float*)
* `$alternate` is whether the direction of the animation alternates, making the animation turn back on itself (*boolean, false by default*)
* `$iterations` is the number of times the animation happens, based on `animation-iteration-count` (*integer, but "infinite" by default*)

### More complex example

```
.animation-name-2 {
	@include cel-animation((3 2 3 1), 0.1, true, 2);
}
```

In this example, there are four cels representing four child elements in the `animation-name-2` container. The first cel is visible for three frames at the beginning of the animation, the second for two frames after that and so on. The frame rate is 0.1 seconds and the animation alternates (reverses back on itself) twice before stopping.

## Demo

![Animated Shark](http://heydonworks.com/SVG_animations/sharky.svg)

**Note: In some browsers, you'll need to [go to the actual SVG](http://heydonworks.com/SVG_animations/sharky.svg) to see this demo animate. This is because I've used the `<img/>` tag here, which doesn't always honor embedded CSS. To make it work in a real situation, either inline the SVG or include it using `<object>`. Thank you to Sara Soueidan for her advice here.**

My shark SVG is animated using two frame animations on SVG `<g>` elements, set out as follows. Note that the `.eyes` animation has the open eye set at 6 frames. Both animations alternate.

```
.tail {
  @include cel-animation((1 1 1), 0.2, true);
}

.eyes {
  @include cel-animation((6 1 1 1), 0.1, true);
}
```
