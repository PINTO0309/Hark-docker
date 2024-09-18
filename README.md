# Hark-docker
Building and installing Hark from source code and building a Docker Image.

![image](https://github.com/user-attachments/assets/dc3d1be0-de1e-454d-a39c-eed1f4ea5475)

https://hark.jp/

HARK is open-sourced robot audition software consisting of sound source localization modules, sound source separation modules and automatic speech recognition modules of separated speech signals that work on any robot with any microphone configuration.

Since a robot with ears may be deployed to various auditory environments, the robot audition system should provide an easy way to adapt to them. HARK provides a set of modules to cope with various auditory environments by using an open-sourced middleware, FlowDesigner and reduces the overheads of data transfer between modules.

HARK has been open-sourced since April 2008. The resulting implementation of HARK with MUSIC-based sound source localization, GHDSS-based sound source separation and Missing-Feature-Theory-based automatic speech recognition (ASR) on several robots like HRP-2, SIG, SIG2, and Robovie R2 attains recognizing three simultaneous utterances in real time.

## 1. docker pull
```bash
docker pull pinto0309/hark_cpu:3.5.0
```

## 2. docker build
```bash
docker build \
-t pinto0309/hark_cpu:3.5.0 \
-f Dockerfile.cpu .
```

## 3. docker run
```bash
sudo chmod -R 777 /dev/snd
sudo rm -rf ~/.config/pulse
mkdir -p ~/.config/pulse
pactl load-module module-native-protocol-tcp auth-anonymous=1

IP_ADDRESS=$(hostname -I | cut -f1 -d' ')
docker run --rm -it \
--net host \
--ipc host \
-v ~/.config/pulse:/root/.config/pulse \
-e PULSE_SERVER=$IP_ADDRESS \
pinto0309/hark_cpu:3.5.0
```

## 4. device check
```bash
# Microphone side
pactl list sources short

0 alsa_output.pci-0000_01_00.1.hdmi-stereo.monitor        module-alsa-card.c      s16le 2ch 44100Hz       IDLE
1 alsa_input.usb-AONI_ELECTRONIC_CO._Full_HD_webcam_AN202007180003-02.mono-fallback       module-alsa-card.c      s16le 1ch 44100Hz       SUSPENDED
2 alsa_output.usb-FongLun_USB_Microphone_201605-00.analog-stereo.monitor  module-alsa-card.c      s16le 2ch 44100Hz       SUSPENDED
3 alsa_input.usb-FongLun_USB_Microphone_201605-00.analog-stereo   module-alsa-card.c      s16le 2ch 44100Hz       SUSPENDED
4 alsa_output.pci-0000_00_1f.3.iec958-stereo.monitor      module-alsa-card.c      s16le 2ch 44100Hz       SUSPENDED

# Speaker side
pactl list sinks short

0 alsa_output.pci-0000_01_00.1.hdmi-stereo        module-alsa-card.c      s16le 2ch 44100Hz       SUSPENDED
1 alsa_output.usb-FongLun_USB_Microphone_201605-00.analog-stereo  module-alsa-card.c      s16le 2ch 44100Hz       SUSPENDED
2 alsa_output.pci-0000_00_1f.3.iec958-stereo      module-alsa-card.c      s16le 2ch 44100Hz       SUSPENDED
```

## 5. cited
1. https://hark.jp/
2. https://github.com/kaldi-asr/kaldi

## 6. refs
1. [Hark 3.5.0 のソースコードからのビルドとインストールとDocker Imageビルド](https://zenn.dev/pinto0309/scraps/072fe73c6011c8)
2. https://github.com/PINTO0309/kaldi