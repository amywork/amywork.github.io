---
layout: post
title: "Playing Video"
author: "younari"
---

> [Brian Voong :: Let's Bild That App](https://www.letsbuildthatapp.com/course/YouTube) 튜토리얼을 통해, YoutubeApp 처럼 Video를 Control (재생, 정지, progress) 하는 기능 구현해보기

# 🎬 Core Process

- `import AVFoundation` 
- `AVPlayer`
- `PeriodicTimeObserver`
- `handlePause()`
- `handleSliderChange()`
- `observeValue()`


# AVPlayer
- URL을 통해 `AVPlayer` 만들기

```
let urlString = "https://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4"
if let url = URL(string: urlString) {
	player = AVPlayer(url: url)
}
```

# PeriodicTimeObserver
- `player?.addPeriodicTimeObserver`
- `progressTime`(CMTime) 을 tracking 해서 Int -> String으로 바꿔, `currentTimeLabel`과 `videoSlider`의 값을 조정한다.

```
let interval = CMTime(value: 1, timescale: 2)
player?.addPeriodicTimeObserver(forInterval: interval,
                                queue: DispatchQueue.main,
                                using: { (progressTime) in
	                                    
	                                    // Label : track player progress
	                                    let seconds = CMTimeGetSeconds(progressTime)
	                                    let secondsString = String(format: "%02d", Int(seconds.truncatingRemainder(dividingBy: 60)))
	                                    let minutesString = String(format: "%02d", Int(seconds / 60))
	                                    self.currentTimeLabel.text = "\(minutesString):\(secondsString)"
	                                    
	                                    // Moving Slider
	                                    if let duration = self.player?.currentItem?.duration {
	                                        let durationSeconds = CMTimeGetSeconds(duration)
	                                        self.videoSlider.value = Float(seconds / durationSeconds)
	                                    }
									})
```

# handlePause()
- `isPlaying: Bool` 값에 따라 재생 / 정지 control

```
var isPlaying = false
    
func handlePause() {
    if isPlaying {
        player?.pause()
        pausePlayButton.setImage(UIImage(named: "play"), for: UIControlState())
    } else {
        player?.play()
        pausePlayButton.setImage(UIImage(named: "pause"), for: UIControlState())
    }
    
    isPlaying = !isPlaying
}
```


# handleSliderChange()
- `Slider` 값 조정에 따라 `player: AVPlayer`도 `seek to Time` 하기


```
slider.addTarget(self, action: #selector(handleSliderChange), for: .valueChanged)

func handleSliderChange() {
    if let duration = player?.currentItem?.duration {
        let totalSeconds = CMTimeGetSeconds(duration)
        let value = Float64(videoSlider.value) * totalSeconds
        let seekTime = CMTime(value: Int64(value), timescale: 1)
        player?.seek(to: seekTime, completionHandler: { (completedSeek) in
       		// Do something!
        })
    }
}
```

# observeValue()
- `AVPlayer`의 instance에 특정 String 값으로 `addObserver()`

```
player?.addObserver(self, forKeyPath: "currentItem.Loaded", options: .new, context: nil)
```

- `override func observeValue()`를 통해 player의 상태에 따라 `activityIndicatorView`, `isPlaying` 값 등을 변경한다.

```
override func observeValue(forKeyPath keyPath: String?,
                           of object: Any?,
                           change: [NSKeyValueChangeKey : Any]?,
                           context: UnsafeMutableRawPointer?) {
    
    if keyPath == "currentItem.Loaded" {
        activityIndicatorView.stopAnimating()
        controlsContainerView.backgroundColor = .clear
        pausePlayButton.isHidden = false
        isPlaying = true
        
        if let duration = player?.currentItem?.duration {
            let seconds = CMTimeGetSeconds(duration)
            
            let secondsText = Int(seconds) % 60
            let minutesText = String(format: "%02d", Int(seconds) / 60)
            videoLengthLabel.text = "\(minutesText):\(secondsText)"
        }
    }
    
}
```


# UI Frame Sample
- `16:9 Frame`으로 VideoPlayerView 잡아주기

```
let height = keyWindow.frame.width * 9 / 16
let videoPlayerFrame = CGRect(x: 0, y: 0, width: keyWindow.frame.width, height: height)
let videoPlayerView = VideoPlayerView(frame: videoPlayerFrame)
```

- Slider 백그라운드에 Gradient를 깔아주고 싶을 경우 : **`setupGradientLayer()`**

```
fileprivate func setupGradientLayer() {
    let gradientLayer = CAGradientLayer()
    gradientLayer.frame = bounds
    gradientLayer.colors = [UIColor.clear.cgColor, UIColor.black.cgColor]
    gradientLayer.locations = [0.7, 1.0]
    controlsContainerView.layer.addSublayer(gradientLayer)
}
```