# Frame Based Animation
A Sass @mixin for creating traditional frame-based animations, especially with SVG. Think gifs, but scalable and with more control over combining animations together.

## Install

Either simply download the `_frame-based-animation.scss` partial from this repo' or use Bower as below:

```
bower install --save-dev frame-based-animation
```

## Include in your project

`@include 'frame-based-animation'`

## Usage

### The markup

First you need to choose a (class) name for your animation and include the "frame" elements within this named container. These can be any type of SVG or HTML elements. You should put them in the order you want them animated.

In the following code, we are using the generic "animation-name" class on an SVG `<g>` element and our frames are `<path>` elements.

```
<g class="animation-name">
	<path d="[path coords for first frame]"></path>
	<path d="[path coords for second frame]"></path>
	<path d="[path coords for third frame]"></path>
</g>

```

### Basic example

In the this basic example, we are including the only mandatory parameter, `$framecount`, which should always be equal to the number of frame elements. In this case it's three (to match the number of path elements above).

```
.animation-name {
	@include frame-animation(3);
}

```

This will create a frame-based animation that shows each frame in order, then in reverse order (the direction alternates using `animation-direction: alternate`) *ad infinitum*. The frame rate defaults to `0.25`, making each iteration of the animation 0.25 * 3 * 2 seconds. The 2 is because the alternation doubles the length of each iteration.

### All parameters

Only the `$framecount` parameter is required. The others, should you wish to use them, should be included in the order they are below.

* `$framecount` is the number of frames, which must match the number of child elements (*integer*)
* `$framerate` is the duration of each frame's appearance. Eg. 0.25 (the default) means 0.25 frames per second (fps) (*float*)
* `$alternate` is whether the direction of the animation alternates, making the animation turn back on itself (*boolean, true by default*)
* `$iterations` is the number of times the animation happens, based on `animation-iteration-count` (*integer, but "infinite" by default*)

### More complex example

```
.animation-name {
	@include frame-animation(12, 0.1, false, 2);
}
```

In this example, there are 12 frames, the frame rate is 0.1 frames per second (fps) and the animation direction does not alternate. It iterates twice, then stops.

## Demo

I've created [a little demo](http://heydonworks.com/frame-animation-demos/stamp-dance.html) using one inline SVG incorporating three concurrent animations, all using the `frame-animation` `@mixin`. The SVG was created in Inkscape, and&mdash;with the frames placed on top of each other in their groups&mdash; looks like this when first created:

![Aggressive dancer](http://heydonworks.com/frame-animation-demos/stamp-dance.png)

The Sass that takes this SVG and its frame groups, turning them into the intended animation, is like this:

```
.arms {
  @include frame-animation(3, 0.1);
}

.leg {
 @include frame-animation(3, 0.15); 
}

.eyes {
  @include frame-animation(5, 0.12); 
}
```

Note that the different frame rates for each separate animation make for a more complex composite animation, not possible with gifs which are just _an_ animation. The generated CSS looks like this:

```
.arms > * {
  opacity: 0;
  animation-duration: 0.27s;
  animation-direction: alternate;
  animation-iteration-count: infinite;
  animation-timing-function: steps(1);
}

@keyframes arms-1 {
  0% {
    opacity: 1;
  }
  33.33333% {
    opacity: 0;
  }
}

.arms > :nth-child(1) {
  animation-name: arms-1;
}

@keyframes arms-2 {
  33.33333% {
    opacity: 1;
  }
  66.66667% {
    opacity: 0;
  }
}

.arms > :nth-child(2) {
  animation-name: arms-2;
}

@keyframes arms-3 {
  66.66667% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

.arms > :nth-child(3) {
  animation-name: arms-3;
}

.leg > * {
  opacity: 0;
  animation-duration: 0.45s;
  animation-direction: alternate;
  animation-iteration-count: infinite;
  animation-timing-function: steps(1);
}

@keyframes leg-1 {
  0% {
    opacity: 1;
  }
  33.33333% {
    opacity: 0;
  }
}

.leg > :nth-child(1) {
  animation-name: leg-1;
}

@keyframes leg-2 {
  33.33333% {
    opacity: 1;
  }
  66.66667% {
    opacity: 0;
  }
}

.leg > :nth-child(2) {
  animation-name: leg-2;
}

@keyframes leg-3 {
  66.66667% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

.leg > :nth-child(3) {
  animation-name: leg-3;
}

.eyes > * {
  opacity: 0;
  animation-duration: 0.6s;
  animation-direction: alternate;
  animation-iteration-count: infinite;
  animation-timing-function: steps(1);
}

@keyframes eyes-1 {
  0% {
    opacity: 1;
  }
  20% {
    opacity: 0;
  }
}

.eyes > :nth-child(1) {
  animation-name: eyes-1;
}

@keyframes eyes-2 {
  20% {
    opacity: 1;
  }
  40% {
    opacity: 0;
  }
}

.eyes > :nth-child(2) {
  animation-name: eyes-2;
}

@keyframes eyes-3 {
  40% {
    opacity: 1;
  }
  60% {
    opacity: 0;
  }
}

.eyes > :nth-child(3) {
  animation-name: eyes-3;
}

@keyframes eyes-4 {
  60% {
    opacity: 1;
  }
  80% {
    opacity: 0;
  }
}

.eyes > :nth-child(4) {
  animation-name: eyes-4;
}

@keyframes eyes-5 {
  80% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

.eyes > :nth-child(5) {
  animation-name: eyes-5;
}
```

Note the use of identical drawings in 2 of the 5 `.eyes` frames, to create the illusion of a small pause in animation.
