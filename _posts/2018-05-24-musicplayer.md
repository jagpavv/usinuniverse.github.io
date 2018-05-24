---
title: 뮤직 플레이어
layout: post
hide: true
---

# 일곱번째 연습 - 뮤직 플레이어(커넥트 재단 - iOS 부스트 코스)

### UI 구현

[![Chatting](https://github.com/usinuniverse/usinuniverse.github.io/blob/master/assets/images/project%20images/08/01.png?raw=true)](https://vimeo.com/271657271)

* 클릭하시면 동영상을 감상하실 수 있습니다.

### 요구사항

1. 화면구성
* 재생 버튼, 타임 레이블, 슬라이더가 화면 X축 중앙에 위치합니다.
* 각각의 요소는 서로의 영역을 침범하거나 겹치지 않으며 화면 밖으로 나가지 않도록 합니다.
* 타임 레이블과 슬라이더는 0에서 시작합니다.

2. 기능
* 재생 버튼을 누르면 음악을 재생하고, 일시정지 버튼으로 바뀌며 재생위치에 따라 슬라이더가 움직입니다.
* 일시정지 버튼을 누르면 음악을 멈추고, 재생 버튼으로 바뀌며 슬라이더 움직임이 정지합니다.
* 음악이 재생됨에 따라 타임 레이블이 밀리세컨드 단위(0.01초)로 변경됩니다.
* 음악이 재생됨에 따라 타임 레이블과 슬라이더의 값이 밀리세컨드 단위(0.01초)로 변경됩니다.
* 슬라이더의 위치를 변경해 현재 재생위치를 조절할 수 있습니다.
* 음악을 모두 재생하면 버튼, 레이블, 슬라이더가 맨 처음 상태로 되돌아갑니다.

### 코드

```swift
import UIKit
import AVFoundation



class ViewController: UIViewController {
    
    // MARK: - Properties
    // MARK: Custom
    
    var player: AVAudioPlayer!
    var timer: Timer!
    
    // MARK: IBOutlet
    
    @IBOutlet weak var playPuaseButton: UIButton!
    @IBOutlet weak var timeLabel: UILabel!
    @IBOutlet weak var progressSlider: UISlider!
    
    
    
    // MARK: - Methods
    // MARK: View Life Cycle
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        initializePlayer()
        initializeProgressSlider()
    }
    
    // MARK: Memory Management
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    // MARK: Custom
    
    // player 셋업
    func initializePlayer() {
        guard let soundAsset = NSDataAsset(name: "sound") else {
            print("Asset 불러오기 실패")
            return
        }
        
        do {
            try self.player = AVAudioPlayer(data: soundAsset.data)
            self.player.delegate = self
        } catch {
            print(error)
        }
    }
    
    // progressSlider 셋업
    func initializeProgressSlider() {
        self.progressSlider.minimumValue = 0
        self.progressSlider.maximumValue = Float(self.player.duration)
        self.progressSlider.value = self.progressSlider.minimumValue
    }
    
    // progressSlider 값과 timeLabel의 값 싱크 맞추기
    func syncTimeLabelText(with time: TimeInterval) {
        let minute = Int(time / 60)
        let second = Int(time.truncatingRemainder(dividingBy: 60))
        let milisecond = Int(time.truncatingRemainder(dividingBy: 1) * 100)
        
        let timeLabelText = String(format: "%02ld:%02ld:%02ld", minute, second, milisecond)
        
        self.timeLabel.text = timeLabelText
    }
    
    // 타이머 생성
    func makeAndFireTimer() {
        self.timer = Timer.scheduledTimer(withTimeInterval: 0.01, repeats: true, block: { [unowned self] (timer: Timer) in
            if self.progressSlider.isTracking { return }
            
            self.syncTimeLabelText(with: self.player.currentTime)
            self.progressSlider.value = Float(self.player.currentTime)
        })
        self.timer.fire()
    }
    
    // 타이머 초기화
    func invalidateTimer() {
        self.timer.invalidate()
        self.timer = nil
    }
    
    // MARK: IBAction
    
    // playPuaseButton이 눌릴 때 실행
    @IBAction func playPauseButtonDidTap(_ sender: UIButton) {
        sender.isSelected = !sender.isSelected
        
        if sender.isSelected {
            self.player.play()
            self.makeAndFireTimer()
        } else {
            self.player.pause()
            self.invalidateTimer()
        }
    }
    
    // progressSlider의 값이 변경될 때 실행
    @IBAction func progressSliderDidSlide(_ sender: UISlider) {
        self.syncTimeLabelText(with: TimeInterval(sender.value))
        
        if sender.isTracking { return }
        
        self.player.currentTime = TimeInterval(sender.value)
    }
    
}

// MARK: - Extension
// MARK: AVAudio Player Delegate

extension ViewController: AVAudioPlayerDelegate {
    
    // 플레이가 끝나면 playPuaseButton, progressSlider 등 UX 적인 요소들 변경
    func audioPlayerDidFinishPlaying(_ player: AVAudioPlayer, successfully flag: Bool) {
        self.playPuaseButton.isSelected = false
        self.progressSlider.value = 0
        self.syncTimeLabelText(with: 0)
        self.invalidateTimer()
    }
    
}



```
