# Jetter Buffer

##### 为什么在rtp传输的接收端要用buffer来接受数据？

* 在音视频传输的过程中为了保证音视频播放的流畅性，通常会选择缓冲一部份数据后在开始播放
* 在rtp的接收端收到的包是乱序的，为了获取到正确的数据也需要利用缓冲区

