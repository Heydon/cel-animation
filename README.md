# Frame Based Animation
A Sass @mixin for creating traditional frame-based animations, especially with SVG. The @mixin iterates over sibling elements, making them frames in an animated sequence.

## Include in your project

`@include 'frame-based-animation'`

## Usage

### The markup

First you need to choose a (class) name for your animation and include the "frame" elements within it. These can be any type of SVG or HTML elements. You should put them in the order you want them animated.

In the following example, we are using the generic "animation-name" class on an SVG `<g>` element and our frames are `<path>` elements.

```
<g class="animation-name">
	<path d="[path coords for first frame]"></path>
	<path d="[path coords for second frame]"></path>
	<path d="[path coords for third frame]"></path>
</g>

```

### Basic example

In the following basic example, we are including the only mandatory parameter, `$framecount`, which should always be equal to the number of frame elements - in this case, three (see above).

```
.animation-name {
	@include frame-animation(3);
}

```

This will create a frame-based animation that shows each frame in order, then in reverse order (the direction alternates using `animation-direction: alternate`) *ad infinitum*. The frame rate defaults to `0.25`, making each iteration of the animation 0.25 * 3 * 2 seconds. The 2 is because the alternation doubles the length of each iteration.

### All parameters

Only the `$framecount` parameter is required. The others, should you wish to use them, should be included in the order they are below.

* `$framecount` is the number of frames, which must match the number of child elements (*integer*)
* `$framerate` is the duration of each frame's appearance. Eg. 0.25 (the default) means 0.25 frames per second (*float*)
* `$alternate` is whether the direction of the animation alternates, making the animation turn back on itself (*boolean, true by default*)
* `$iterations` is the number of times the animation happens, based on `animation-iteration-count` (*integer, but "infinite" by default*)

### More complex example

.animation-name {
	@include frame-animation(12, 0.1, false, 2);
}

In this example, there are 12 frames, the frame rate is 0.1 frames per second (fps) and the animation direction does not alternate. It iterates twice, then stops.

