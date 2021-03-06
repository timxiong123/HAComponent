本组件连接PulseAudio服务，进行声音播放

将以下内容放置在`configuration.yaml`文件中：
```yaml
# configuration.yaml样例
media_player:
  - platform: pulseaudio
    name: xxxxxx
    sink: alsa_output.1.stereo-fallback
```
可配置项：
- **name** （*可选项*）: media_player的实体名，缺省值为`Pulse Audio Speaker`
- **sink** (*可选项*): PulseAudio服务中音频输出设备，可以通过命令`pactl list sinks`查看系统中所有的sink。sink缺省为`default`，表示使用系统的缺省音频输出设备。

你也可以在前端`集成`中进行配置，但首先需要修改HomeAssistant源代码`homeassistant/generated/config_flows.py`，在其中添加`"pulseaudio",`

## 在hassio中配置蓝牙音箱

1. [进入主操作系统](https://developers.home-assistant.io/docs/operating-system/debugging)

2. 在PulseAudio中增加bluetooth模块
  `docker exec hassio_audio pactl load-module module-bluetooth-discover`

3. 蓝牙音箱连接，运行命令`bluetoothctl`

  ```
  scan on
  pair <your mac address>
  trust <your mac address>
  connect [enter your MAC add]
  discoverable on
  pairable on
  agent NoInputNoOutput
  default-agent 
  ```

4. 在前端集成中配置，或者在`configuration.yaml`中配置。如果在`configuration.yaml`中配置，sink的名称可以通过命令`docker exec hassio_audio pactl list sinks`查看

注：如果你不是在HASSOS上运行hassio-supervisor，你可能需要运行以下命令，去除Host操作系统上的`pulseaudio`和`bluealsa`，因为它们会首先截获蓝牙设备链接信息。
```
sudo apt-get remove pulseaudio
sudo apt-get remove bluealsa
sudo reboot
```
