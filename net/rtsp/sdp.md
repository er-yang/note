# SDP

sdp(Session Description Protocol)是一种会话描述协议，用来表述会话之间交换的信息，特别的用来在多媒体信息传输之间来传递，session announcement, session invitation和其他形式的媒体信息

## 内容

一份SDP形式的内容通常会包含一下几种信息

* Session name and purpose
* Time(s) the session is active
* The media comprising the session
*  Information to receive those media (addresses, ports, formats and
       so on)

由于参加会议所需的资源可能有限， 可能还需要一些其他信息

* Information about the bandwidth to be used by the conference
*  Contact information for the person responsible for the session

## 媒体信息

媒体信息通常会包含媒体类型，传输协议，以及目标地址、端口等信息

eg：


*  The type of media (video, audio, etc)
*  The transport protocol (RTP/UDP/IP, H.320, etc)
*   The format of the media (H.261 video, MPEG video, etc)

   For an IP multicast session, the following are also conveyed:

* Multicast address for media
* Transport Port for media

## 字段描述

可选项带*号
```
Session description
        v=  (protocol version)
        o=  (owner/creator and session identifier).
        s=  (session name)
        i=* (session information)
        u=* (URI of description)
        e=* (email address)
        p=* (phone number)
        c=* (connection information - not required if included in all media)
        b=* (bandwidth information)
        One or more time descriptions (see below)
        z=* (time zone adjustments)
        k=* (encryption key)
        a=* (zero or more session attribute lines)
        Zero or more media descriptions (see below)

Time description
        t=  (time the session is active)
        r=* (zero or more repeat times)

Media description
        m=  (media name and transport address)
        i=* (media title)
        c=* (connection information - optional if included at session-level)
        b=* (bandwidth information)
        k=* (encryption key)
        a=* (zero or more media attribute lines)
```