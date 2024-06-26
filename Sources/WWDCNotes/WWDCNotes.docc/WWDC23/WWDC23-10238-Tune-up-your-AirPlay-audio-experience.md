# Tune up your AirPlay audio experience

Learn how you can upgrade your app’s AirPlay audio experience to be more robust and responsive. We’ll show you how to adopt enhanced audio buffering with AVQueuePlayer, explore alternatives when building a custom player in your app, and share best practices.

@Metadata {
   @TitleHeading("WWDC23")
   @PageKind(sampleCode)
   @CallToAction(url: "https://developer.apple.com/wwdc23/10238", purpose: link, label: "Watch Video (10 min)")

   @Contributors {
      @GitHubUser(RamitSharma991)
   }
}



## Airplay Overview
- With AirPlay Audio you can stream your favorite music or podcast in perfect sync. 
- With AirPlay Video, you can stream your favorite movies and shows.
- With mirroring, you can share photos, personal videos, games, web pages, or spreadsheets. 

## Airplay Enhanced Audio Buffering
 - Built with a new and improved protocol keeping Whole home audio in mind.
 - Audio streams faster than real-time playback speed in order to minimize playback interruptions. 
 - Highly responsive on HomePod or iPhone as a remote control.
 - Supports multi-channel audio formats, like Dolby ATMOS.
 - Intelligent use of Lossless playback for iOS.
 - Supports HLS Interstitials. 

## Add Support To Your App
 - If playing media is central to your app, set your audio session's category to `.playback`. This will ensure your app's media will continue playing when the app is in background.

```swift 
let audioSession = AVAudioSession.sharedInstance()
try audioSession.setCategory(. playback ,xmode: . default , policy:.longFormAudio ) 
```
- for spoken audio, like podcasts or audiobooks, to set the mode to `.spokenAudio`
- set the audio session's routing policy to `.longFormAudio`. Longform audio is anything other than system sounds, such as music or podcasts. 

## Intelligent Airplay Support 
- Add a new key/value to `info.plist` and set `AVInitialRouteSharingPolicy = LongformAudio`

### Supporting Airplay
- Identify the audio type: add `AVRoutePickerView` to your view hierarchy to include an AirPlay picker in your app. 
- Add an Airplay picker: The picker provides people with a list of potential AirPlay devices that they can use with your app.
- Add a media player: `MPNowPlayingInfoCenter` and `MPRemoteCommandCenter` to receive remote commands, like play, pause, or skip.

## Supporting Airplay Enhanced Audio Buffering

`AVPLayer` or `AVQueuePlayer` is the simplest way to support enhanced audio buffering for your app where AVQueuePlayer is highly recommended.
- Create a queue player `let player = AVQueuePlayer()`
- Identify a URL that points to local or cloud content that you want to play.
- Then create an AVAsset instance with the URL, and create an AVPlayerItem instance with that asset.
- Give the AVPlayerItem to the player and start playback.
- Automatically gets enhanced audio buffering when it is routed to AirPlay.

```swift
let player = AVQueuePlayer()

let url = URL(string: "http://www.examplecontenturl.com")
let asset = AVAsset(url: url)
let item = AVPlayItem(asset: asset)

player.insert(item, after: nil)
player.play()

```
- If your app preprocess on the media data or have a DRM model AVPlayer use`AVSampleBufferAudioRenderer and AVSampleBufferRenderSynchronizer` to synchronize multiple queued sample buffers to a single timeline
- create a serial queue to perform all playback operations on. 
- Create the audio renderer and the render synchronizer. The synchronizer is used to establish the media timeline.

```swift
let serializationQueue = DispatchQueue(label: "sample.buffer.player.serialization.queue")
let audioRenderer = AVSampleBufferAudioRenderer()
let renderSynchronizer = AVSampleBufferRenderSynchronizer()
```
- Add the audio renderer to the render synchronizer `renderSynchronizer.addRenderer(audioRenderer)`. This will tell the audio renderer to follow the media timeline.
- To enqueue audio data, install a callback that will let you know you need more data.

```swift
serializationQueue.async { [weak self] in
    guard let self = self else { return }
    // Start processing audio data and stop when there's no more data.
    self.audioRenderer.requestMediaDataWhenReady(on: serializationQueue) { [weak self] in
        guard let self = self else { return }
        while self.audioRenderer.isReadyForMoreMediaData {
            let sampleBuffer = self.nextSampleBuffer() // Returns nil at end of data.
            if let sampleBuffer = sampleBuffer {
                self.audioRenderer.enqueue(sampleBuffer)
            } else {
                // Tell the renderer to stop requesting audio data.
                audioRenderer.stopRequestingMediaData()
            }
        }
    }

    // Start playback at the natural rate of the media.
    self.renderSynchronizer.rate = 1.0
}
```
- Both APIs will work for non-AirPlay playback, including local or Bluetooth. 
- Developers might want different APIs for AirPlay and non-AirPlay playback. In that case, your app can register to the routeChangeNotification and act accordingly depending on the current route.

## CarPlay enhanced audio Buffering
- car manufacture support.
- Wireless, robust and responsive playback.
- Same APIs support CarPlay.
