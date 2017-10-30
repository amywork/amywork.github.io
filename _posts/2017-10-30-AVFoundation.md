---
layout: post
title: "AV Foundation"
author: "younari"
---
> AV Foundation 도큐멘트 직접 읽고 유용한 프로퍼티 및 메소드 찾아보기

# AVFoundation
- [AVFoundation](https://developer.apple.com/documentation/avfoundation)
- Audio와 Video를 play하기 위한 FrameWork

# AVAsset 
- [AVAsset](https://developer.apple.com/documentation/avfoundation/avasset)
- AVAsset의 모델링된 데이터로 -> AVPlayerItem를 만들고 -> AVPlayer가 재생하는 구조

# Sample Code

{% highlight swift %}

var isPlaying: Bool = false
var player:AVPlayer = AVPlayer()    
    
if let url = currentSong.songURL
{
    let asset = AVAsset(url: url)
    let playerItem = AVPlayerItem(asset: asset, automaticallyLoadedAssetKeys: nil)
    player.replaceCurrentItem(with: playerItem)
    if isPlaying {
        player.play()
        isPlaying = true
    }
}
{% endhighlight %}


# AVPlayer
- [AVPlayer](https://developer.apple.com/documentation/avfoundation/avplayer)
- **URL**로 init하거나, **AVPlayerItem**으로 init하는 방식
- An AVPlayer is a controller object used to manage the playback and timing of a media asset. It provides the interface to **control the player’s transport behavior such as its ability to play, pause, change the playback rate, and seek to various points in time within the media’s timeline.** You can use an AVPlayer to play local and remote file-based media, such as QuickTime movies and MP3 audio files, as well as audiovisual media served using HTTP Live Streaming.
- AVPlayer is intended for playing **a single media asset at a time**. 
- The player instance can be reused to play additional media assets using its **replaceCurrentItem(with:)** method, but it manages the playback of only a single media asset at a time. The framework also provides a subclass of AVPlayer, called **AVQueuePlayer**, that you can use to **create and manage of queue of media assets to be played sequentially**.
- **playback rate**: 재생 속도
- **func currentTime()**: Returns the current time of the current player item.
- You use an AVPlayer to play media assets, which AVFoundation models using the AVAsset class. AVAsset only models the static aspects of the media, such as its duration or creation date, and on its own, is unsuitable for playback with an AVPlayer. To play an asset, you need to create an instance of its dynamic counterpart found in AVPlayerItem.


## [M]func play()
- Begins playback of the current item.

## [M]func pause()
Pauses playback of the current item.

## [P]var rate: Float
- The current playback rate.

## [P]var actionAtItemEnd: AVPlayerActionAtItemEnd
- The action to perform when the current player item has finished playing.

## [M]func replaceCurrentItem(with: AVPlayerItem?)
- Replaces the current player item with a new player item.

### AVPlayer: 자신의 상태가 계속해서 바뀌는 동적 객체로, state를 observe하기 위한 두가지 Method가 존재한다.

## [M]addPeriodicTimeObserver(forInterval:queue:using:)
- https://developer.apple.com/documentation/avfoundation/avplayer/1385829-addperiodictimeobserver

## [M]addBoundaryTimeObserver(forTimes:queue:using:)
- https://developer.apple.com/documentation/avfoundation/avplayer/1388027-addboundarytimeobserver




### cf. AVPlayerActionAtItemEnd (Enum)
- You use these constants with actionAtItemEnd to indicate the action a player should take when it finishes playing.