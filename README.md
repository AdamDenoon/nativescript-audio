<a align="center" href="https://www.npmjs.com/package/nativescript-audio-player">
    <h3 align="center">NativeScript Audio Player</h3>
</a>
<h4 align="center">NativeScript plugin to play audio files for Android and iOS.</h4>

<p align="center">
    <a href="https://www.npmjs.com/package/nativescript-audio-player">
        <img src="https://img.shields.io/npm/v/nativescript-audio-player.svg" alt="npm">
    </a>
    <a href="https://www.npmjs.com/package/nativescript-audio-player">
        <img src="https://img.shields.io/npm/dt/nativescript-audio-player.svg?label=npm%20downloads" alt="npm">
    </a>
    <a href="https://github.com/adamdenoon/nativescript-audio-player/stargazers">
        <img src="https://img.shields.io/github/stars/adamdenoon/nativescript-audio-player.svg" alt="stars">
    </a>
     <a href="https://github.com/adamdenoon/nativescript-audio-player/network">
        <img src="https://img.shields.io/github/forks/adamdenoon/nativescript-audio-player.svg" alt="forks">
    </a>
    <a href="https://github.com/adamdenoon/nativescript-audio-player/blob/master/LICENSE.md">
        <img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="license">
    </a>
    <a href="https://paypal.me/adamdenoon">
        <img src="https://img.shields.io/badge/Donate-PayPal-green.svg" alt="donate">
    </a>
</p>

---

## Installation

`tns plugin add nativescript-audio-player`

---

### Native Classes

* [Android - android.media.MediaPlayer](http://developer.android.com/reference/android/media/MediaPlayer.html)
* [iOS - AVAudioPlayer](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)

## Usage

### TypeScript Example

```typescript
import { TNSPlayer } from 'nativescript-audio-player';

export class YourClass {
  private _player: TNSPlayer;

  constructor() {
    this._player = new TNSPlayer();
    this._player.debug = true; // set true to enable TNSPlayer console logs for debugging.
    this._player
      .initFromFile({
        audioFile: '~/audio/song.mp3', // ~ = app directory
        loop: false,
        completeCallback: this._trackComplete.bind(this),
        errorCallback: this._trackError.bind(this)
      })
      .then(() => {
        this._player.getAudioTrackDuration().then(duration => {
          // iOS: duration is in seconds
          // Android: duration is in milliseconds
          console.log(`song duration:`, duration);
        });
      });
  }

  public togglePlay() {
    if (this._player.isAudioPlaying()) {
      this._player.pause();
    } else {
      this._player.play();
    }
  }

  private _trackComplete(args: any) {
    console.log('reference back to player:', args.player);
    // iOS only: flag indicating if completed succesfully
    console.log('whether song play completed successfully:', args.flag);
  }

  private _trackError(args: any) {
    console.log('reference back to player:', args.player);
    console.log('the error:', args.error);
    // Android only: extra detail on error
    console.log('extra info on the error:', args.extra);
  }
}
```

### Javascript Example:

```javascript
const audio = require('nativescript-audio-player');

const player = new audio.TNSPlayer();
const playerOptions = {
  audioFile: 'http://some/audio/file.mp3',
  loop: false,
  completeCallback: function() {
    console.log('finished playing');
  },
  errorCallback: function(errorObject) {
    console.log(JSON.stringify(errorObject));
  },
  infoCallback: function(args) {
    console.log(JSON.stringify(args));
  }
};

player
  .playFromUrl(playerOptions)
  .then(function(res) {
    console.log(res);
  })
  .catch(function(err) {
    console.log('something went wrong...', err);
  });
```

## API

### Player

#### TNSPlayer Methods

| Method                                                                 | Description                                                  |
| ---------------------------------------------------------------------- | ------------------------------------------------------------ |
| _initFromFile(options: AudioPlayerOptions)_: `Promise`                 | Initialize player instance with a file without auto-playing. |
| _playFromFile(options: AudioPlayerOptions)_: `Promise`                 | Auto-play from a file.                                       |
| _initFromUrl(options: AudioPlayerOptions)_: `Promise`                  | Initialize player instance from a url without auto-playing.  |
| _playFromUrl(options: AudioPlayerOptions)_: `Promise`                  | Auto-play from a url.                                        |
| _pause()_: `Promise<boolean>`                                          | Pause playback.                                              |
| _resume()_: `void`                                                     | Resume playback.                                             |
| _seekTo(time:number)_: `Promise<boolean>`                              | Seek to position.                                            |
| _dispose()_: `Promise<boolean>`                                        | Free up resources when done playing audio.                   |
| _isAudioPlaying()_: `boolean`                                          | Determine if player is playing.                              |
| _getAudioTrackDuration()_: `Promise<string>`                           | Duration of media file assigned to the player.               |
| _playAtTime(time: number)_: void - **_iOS Only_**                      | Play audio track at specific time of duration.               |
| _changePlayerSpeed(speed: number)_: void - **On Android Only API 23+** | Change the playback speed of the media player.               |

#### TNSPlayer Instance Properties

| Property                | Description                                                |
| ----------------------- | ---------------------------------------------------------- |
| _ios_                   | Get the native ios AVAudioPlayer instance.                 |
| _android_               | Get the native android MediaPlayer instance.               |
| _debug_: `boolean`      | Set true to enable debugging console logs (default false). |
| _currentTime_: `number` | Get the current time in the media file's duration.         |
| _volume_: `number`      | Get/Set the player volume. Value range from 0 to 1.        |

### License

[MIT](/LICENSE)
