# video.js Playlist API

## Playlist Item Object

A playlist is an array of playlist items. A playlist item is an object with the following properties:

| Property     | Type   | Optional | Description                                        |
| ------------ | ------ | -------- | -------------------------------------------------- |
| `sources`    | Array  |          | An array of sources that video.js understands.     |
| `poster`     | String | ✓        | A poster image to display for these sources.       |
| `textTracks` | Array  | ✓        | An array of text tracks that Video.js understands. |

## Methods
### `player.playlist([Array newList], [Number newIndex]) -> Array`

Get or set the current playlist for a player.

If called without arguments, it is a getter. With an argument, it is a setter.

```js
player.playlist([{
  sources: [{
    src: 'http://media.w3.org/2010/05/sintel/trailer.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/sintel/poster.png'
}, {
  sources: [{
    src: 'http://media.w3.org/2010/05/bunny/trailer.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/bunny/poster.png'
}, {
  sources: [{
    src: 'http://vjs.zencdn.net/v/oceans.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://www.videojs.com/img/poster.jpg'
}, {
  sources: [{
    src: 'http://media.w3.org/2010/05/bunny/movie.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/bunny/poster.png'
}, {
  sources: [{
    src: 'http://media.w3.org/2010/05/video/movie_300.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/video/poster.png'
}]);
// [{ ... }, ... ]

player.playlist([{
  sources: [{
    src: 'http://media.w3.org/2010/05/video/movie_300.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/video/poster.png'
}]);
// [{ ... }]
```

If a setter, a second argument sets the video to be loaded. If omitted, the
first video is loaded. If `-1`, no video is loaded.

```js
player.playlist([{
  sources: [{
    src: 'http://media.w3.org/2010/05/sintel/trailer.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/sintel/poster.png'
}, {
  sources: [{
    src: 'http://media.w3.org/2010/05/bunny/trailer.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/bunny/poster.png'
}], 1);
// [{ ... }]
```

#### `player.playlist.currentItem([Number index]) -> Number`

Get or set the current item index.

If called without arguments, it is a getter. With an argument, it is a setter.

If the player is currently playing a non-playlist video, it will return `-1`.

```js
var samplePlaylist = [{
  sources: [{
    src: 'http://media.w3.org/2010/05/sintel/trailer.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/sintel/poster.png'
}, {
  sources: [{
    src: 'http://media.w3.org/2010/05/bunny/trailer.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/bunny/poster.png'
}];

player.playlist(samplePlaylist);

player.currentItem();
// 0

player.currentItem(2);
// 2

player.src('http://example.com/video.mp4');
player.playlist.currentItem();
// -1
```

#### `player.playlist.contains(String|Object|Array value) -> Boolean`

Determine whether a string, source object, or playlist item is contained within a playlist.

Assuming the playlist used above, consider the following example:

```js
player.playlist.contains('http://media.w3.org/2010/05/sintel/trailer.mp4');
// true

player.playlist.contains([{
  src: 'http://media.w3.org/2010/05/sintel/poster.png',
  type: 'image/png'
}]);
// false

player.playlist.contains({
  sources: [{
    src: 'http://media.w3.org/2010/05/sintel/trailer.mp4',
    type: 'video/mp4'
  }]
});
// true
```

#### `player.playlist.indexOf(String|Object|Array value) -> Number`

Get the index of a string, source object, or playlist item in the playlist. If not found, returns `-1`.

Assuming the playlist used above, consider the following example:

```js
player.playlist.indexOf('http://media.w3.org/2010/05/bunny/trailer.mp4');
// 1

player.playlist.contains([{
  src: 'http://media.w3.org/2010/05/bunny/movie.mp4',
  type: 'video/mp4'
}]);
// 3

player.playlist.contains({
  sources: [{
    src: 'http://media.w3.org/2010/05/video/movie_300.mp4',
    type: 'video/mp4'
  }]
});
// 4
```

#### `player.playlist.currentIndex() -> Number`

Get the index of the current item in the playlist. This is identical to calling `currentItem()` with no arguments.

```js
var samplePlaylist = [{
  sources: [{
    src: 'http://media.w3.org/2010/05/sintel/trailer.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/sintel/poster.png'
}, {
  sources: [{
    src: 'http://media.w3.org/2010/05/bunny/trailer.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/bunny/poster.png'
}];

player.currentIndex();
// 0
```

#### `player.playlist.nextIndex() -> Number`

Get the index of the next item in the playlist.

If the player is on the last item, returns the last item's index. However, if the playlist repeats and is on the last item, returns `0`.

If the player is currently playing a non-playlist video, it will return `-1`.

```js
var samplePlaylist = [{
  sources: [{
    src: 'http://media.w3.org/2010/05/sintel/trailer.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/sintel/poster.png'
}, {
  sources: [{
    src: 'http://media.w3.org/2010/05/bunny/trailer.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/bunny/poster.png'
}];

player.playlist(samplePlaylist);

player.nextIndex();
// 1

player.next();
player.nextIndex();
// 1

player.repeat(true);
player.nextIndex();
// 0

player.src('http://example.com/video.mp4');
player.playlist.nextIndex();
// -1
```

#### `player.playlist.previousIndex() -> Number`

Get the index of the previous item in the playlist.

If the player is on the first item, returns `0`. However, if the playlist repeats and is on the first item, returns the last item's index.

If the player is currently playing a non-playlist video, it will return `-1`.

```js
var samplePlaylist = [{
  sources: [{
    src: 'http://media.w3.org/2010/05/sintel/trailer.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/sintel/poster.png'
}, {
  sources: [{
    src: 'http://media.w3.org/2010/05/bunny/trailer.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/bunny/poster.png'
}];

player.playlist(samplePlaylist, 1);

player.previousIndex();
// 0

player.previous();
player.previousIndex();
// 0

player.repeat(true);
player.previousIndex();
// 1

player.src('http://example.com/video.mp4');
player.playlist.previousIndex();
// -1
```

#### `player.playlist.lastIndex() -> Number`

Get the index of the last item in the playlist.

```js
var samplePlaylist = [{
  sources: [{
    src: 'http://media.w3.org/2010/05/sintel/trailer.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/sintel/poster.png'
}, {
  sources: [{
    src: 'http://media.w3.org/2010/05/bunny/trailer.mp4',
    type: 'video/mp4'
  }],
  poster: 'http://media.w3.org/2010/05/bunny/poster.png'
}];

player.lastIndex();
// 1
```

#### `player.playlist.first() -> Object|undefined`

Play the first item in the playlist.

Returns the activated playlist item unless the playlist is empty (in which case, returns `undefined`).

```js
player.playlist.first();
// { ... }

player.playlist([]);
player.playlist.first();
// undefined
```

#### `player.playlist.last() -> Object|undefined`

Play the last item in the playlist.

Returns the activated playlist item unless the playlist is empty (in which case, returns `undefined`).

```js
player.playlist.last();
// { ... }

player.playlist([]);
player.playlist.last();
// undefined
```

#### `player.playlist.next() -> Object`

Advance to the next item in the playlist.

Returns the activated playlist item unless the playlist is at the end (in which case, returns `undefined`).

```js
player.playlist.next();
// { ... }

player.playlist.last();
player.playlist.next();
// undefined
```

#### `player.playlist.previous() -> Object`

Go back to the previous item in the playlist.

Returns the activated playlist item unless the playlist is at the beginning (in which case, returns `undefined`).

```js
player.playlist.next();
// { ... }

player.playlist.previous();
// { ... }

player.playlist.first();
// { ... }

player.playlist.previous();
// undefined
```

#### `player.playlist.autoadvance([Number delay]) -> undefined`

Sets up playlist auto-advance behavior.

Once invoked, at the end of each video in the playlist, the plugin will wait `delay` seconds before proceeding automatically to the next video.

Any value which is not a positive, finite integer, will be treated as a request to cancel and reset the auto-advance behavior.

If you change auto-advance during a delay, the auto-advance will be canceled and it will not advance the next video, but it will use the new timeout value for the following videos.

```js
// no wait before loading in the next item
player.playlist.autoadvance(0);

// wait 5 seconds before loading in the next item
player.playlist.autoadvance(5);

// reset and cancel the auto-advance
player.playlist.autoadvance();
```

#### `player.playlist.repeat([Boolean val]) -> Boolean`

Enable or disable repeat by passing true or false as the argument.

When repeat is enabled, the "next" video after the final video in the playlist is the first video in the playlist. This affects the behavior of calling `next()`, of autoadvance, and so on.

This method returns the current value. Call with no argument to use as a getter.

Examples:

```js

player.playlist.repeat(true);

player.playlist.repeat();
// true

player.playlist.repeat(false);
player.playlist.repeat();
// false

```

#### `player.playlist.sort([Function compare]) -> undefined`

Sort the playlist in a manner identical to [`Array#sort`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort).

Fires the `playlistsorted` event after sorting.

#### `player.playlist.reverse() -> undefined`

Reverse the playlist in a manner identical to [`Array#reverse`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse).

Fires the `playlistsorted` event after reversing.

#### `player.playlist.shuffle() -> undefined`

Shuffles/randomizes the order of playlist items in a manner identical to [`lodash.shuffle`](https://lodash.com/docs/4.17.4#shuffle).

Fires the `playlistsorted` event after shuffling.

## Events

### `playlistchange`

This event is fired asynchronously whenever the contents of the playlist are changed (i.e., when `player.playlist()` is called with an argument).

It is fired asynchronously to let the browser start loading the first video in the new playlist.

```js
player.on('playlistchange', function() {
  player.playlist();
});

player.playlist([]);
// [ ... ]

player.playlist([]);
// [ ... ]
```

### `beforeplaylistitem`

This event is fired before switching to a new content source within a playlist (i.e., when any of `currentItem()`, `first()`, or `last()` is called, but before the player's state has been changed).

### `playlistitem`

This event is fired when switching to a new content source within a playlist (i.e., when any of `currentItem()`, `first()`, or `last()` is called; after the player's state has been changed, but before playback has been resumed).

### `playlistsorted`

This event is fired when any method is called that changes the order of playlist items - `sort()`, `reverse()`, or `shuffle()`.
