Plyr is a simple, lightweight, accessible and customizable HTML5, YouTube and Vimeo media player that supports [_modern_](#browser-support) browsers.



[![Image of Plyr](https://cdn.plyr.io/static/demo/screenshot.png?v=3)](https://plyr.io)

# Features

-   📼 **HTML Video & Audio, YouTube & Vimeo** - support for the major formats
-   💪 **Accessible** - full support for VTT captions and screen readers
-   🔧 **[Customizable](#html)** - make the player look how you want with the markup you want
-   😎 **Clean HTML** - uses the _right_ elements. `<input type="range">` for volume and `<progress>` for progress and well, `<button>`s for buttons. There's no
    `<span>` or `<a href="#">` button hacks
-   📱 **Responsive** - works with any screen size
-   💵 **[Monetization](#ads)** - make money from your videos
-   📹 **[Streaming](#demos)** - support for hls.js, Shaka and dash.js streaming playback
-   🎛 **[API](#api)** - toggle playback, volume, seeking, and more through a standardized API
-   🎤 **[Events](#events)** - no messing around with Vimeo and YouTube APIs, all events are standardized across formats
-   🔎 **[Fullscreen](#fullscreen)** - supports native fullscreen with fallback to "full window" modes
-   ⌨️ **[Shortcuts](#shortcuts)** - supports keyboard shortcuts
-   🖥 **Picture-in-Picture** - supports picture-in-picture mode
-   📱 **Playsinline** - supports the `playsinline` attribute
-   🏎 **Speed controls** - adjust speed on the fly
-   📖 **Multiple captions** - support for multiple caption tracks
-   🌎 **i18n support** - support for internationalization of controls
-   👌 **[Preview thumbnails](#preview-thumbnails)** - support for displaying preview thumbnails
-   🤟 **No frameworks** - written in "vanilla" ES6 JavaScript, no jQuery required
-   💁‍♀️ **SASS** - to include in your build processes

### Demos

You can try Plyr in Codepen using our minimal templates: [HTML5 video](https://codepen.io/pen?template=bKeqpr), [HTML5 audio](https://codepen.io/pen?template=rKLywR), [YouTube](https://codepen.io/pen?template=GGqbbJ), [Vimeo](https://codepen.io/pen?template=bKeXNq). For Streaming we also have example integrations with: [Dash.js](https://codepen.io/pen?template=zaBgBy), [Hls.js](https://codepen.io/pen?template=oyLKQb) and [Shaka Player](https://codepen.io/pen?template=ZRpzZO)

# Quick setup

## HTML

Plyr extends upon the standard [HTML5 media element](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement) markup so that's all you need for those types.

### HTML5 Video

```html
<video poster="/path/to/poster.jpg" id="player" playsinline controls>
    <source src="/path/to/video.mp4" type="video/mp4" />
    <source src="/path/to/video.webm" type="video/webm" />

    <!-- Captions are optional -->
    <track kind="captions" label="English captions" src="/path/to/captions.vtt" srclang="en" default />
</video>
```

### HTML5 Audio

```html
<audio id="player" controls>
    <source src="/path/to/audio.mp3" type="audio/mp3" />
    <source src="/path/to/audio.ogg" type="audio/ogg" />
</audio>
```

For YouTube and Vimeo players, Plyr uses progressive enhancement to enhance the default `<iframe>` embeds. Below are some examples. The `plyr__video-embed` classname will make the embed responsive. You can add the `autoplay`, `loop`, `hl` (YouTube only) and `playsinline` (YouTube only) query parameters to the URL and they will be set as config options automatically. For YouTube, the `origin` should be updated to reflect the domain you're hosting the embed on, or you can opt to omit it.

### YouTube

We recommend [progressive enhancement](https://www.smashingmagazine.com/2009/04/progressive-enhancement-what-it-is-and-how-to-use-it/) with the embedded players. You can elect to use an `<iframe>` as the source element (which Plyr will progressively enhance) or a bog standard `<div>` with two essential data attributes - `data-plyr-provider` and `data-plyr-embed-id`.

```html
<div class="plyr__video-embed" id="player">
    <iframe
        src="https://www.youtube.com/embed/bTqVqk7FSmY?origin=https://plyr.io&amp;iv_load_policy=3&amp;modestbranding=1&amp;playsinline=1&amp;showinfo=0&amp;rel=0&amp;enablejsapi=1"
        allowfullscreen
        allowtransparency
        allow="autoplay"
    ></iframe>
</div>
```

_Note_: The `plyr__video-embed` classname will make the player a responsive 16:9 (most common) iframe embed. When plyr itself kicks in, your custom `ratio` config option will be used.

Or the `<div>` non progressively enhanced method:

```html
<div id="player" data-plyr-provider="youtube" data-plyr-embed-id="bTqVqk7FSmY"></div>
```

_Note_: The `data-plyr-embed-id` can either be the video ID or URL for the media.

### Vimeo

Much the same as YouTube above.

```html
<div class="plyr__video-embed" id="player">
    <iframe
        src="https://player.vimeo.com/video/76979871?loop=false&amp;byline=false&amp;portrait=false&amp;title=false&amp;speed=true&amp;transparent=0&amp;gesture=media"
        allowfullscreen
        allowtransparency
        allow="autoplay"
    ></iframe>
</div>
```

Or the `<div>` non progressively enhanced method:

```html
<div id="player" data-plyr-provider="vimeo" data-plyr-embed-id="76979871"></div>
```

## JavaScript

You can use Plyr as an ES6 module as follows:

```javascript
import Plyr from 'plyr';

const player = new Plyr('#player');
```

Alertnatively you can include the `plyr.js` script before the closing `</body>` tag and then in your JS create a new instance of Plyr as below.

```html
<script src="path/to/plyr.js"></script>
<script>
    const player = new Plyr('#player');
</script>
```

See [initialising](#initialising) for more information on advanced setups.

You can use our CDN (provided by [Fastly](https://www.fastly.com/)) for the JavaScript. There's 2 versions; one with and one without [polyfills](#polyfills). My recommendation would be to manage polyfills seperately as part of your application but to make life easier you can use the polyfilled build.

```html
<script src="https://cdn.plyr.io/3.5.6/plyr.js"></script>
```

...or...

```html
<script src="https://cdn.plyr.io/3.5.6/plyr.polyfilled.js"></script>
```

## CSS

Include the `plyr.css` stylsheet into your `<head>`.

```html
<link rel="stylesheet" href="path/to/plyr.css" />
```

If you want to use our CDN (provided by [Fastly](https://www.fastly.com/)) for the default CSS, you can use the following:

```html
<link rel="stylesheet" href="https://cdn.plyr.io/3.5.6/plyr.css" />

