# youtube-dl-python
youtube-dl-python is a very serious, lightweight, dependency-free Python library (and command-line utility) for downloading YouTube Videos.
Installation
To install from pypi with pip:

$ python -m pip install pytube

Behold, a perfect balance of simplicity versus flexibility:

 >>> YouTube('https://youtu.be/2lAe1cqCOXo').streams.first().download()
 >>> yt = YouTube('http://youtube.com/watch?v=2lAe1cqCOXo')
 >>> yt.streams
  ... .filter(progressive=True, file_extension='mp4')
  ... .order_by('resolution')
  ... .desc()
  ... .first()
  ... .download()

Features

    Support for Both Progressive & DASH Streams
    Support for downloading complete playlist
    Easily Register on_download_progress & on_download_complete callbacks
    Command-line Interfaced Included
    Caption Track Support
    Outputs Caption Tracks to .srt format (SubRip Subtitle)
    Ability to Capture Thumbnail URL.
    Extensively Documented Source Code
    No Third-Party Dependencies
Getting started

Let's begin with showing how easy it is to download a video with pytube:

>>> from pytube import YouTube
>>> YouTube('https://youtube.com/watch?v=2lAe1cqCOXo').streams.first().download()

This example will download the highest quality progressive download stream available.

Next, let's explore how we would view what video streams are available:

>>> yt = YouTube('https://youtube.com/watch?v=2lAe1cqCOXo')
>>> yt.streams
 [<Stream: itag="18" mime_type="video/mp4" res="360p" fps="30fps" vcodec="avc1.42001E" acodec="mp4a.40.2" progressive="True" type="video">,
 <Stream: itag="22" mime_type="video/mp4" res="720p" fps="30fps" vcodec="avc1.64001F" acodec="mp4a.40.2" progressive="True" type="video">,
 <Stream: itag="137" mime_type="video/mp4" res="1080p" fps="30fps" vcodec="avc1.640028" progressive="False" type="video">,
 <Stream: itag="248" mime_type="video/webm" res="1080p" fps="30fps" vcodec="vp9" progressive="False" type="video">,
 <Stream: itag="399" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.08M.08" progressive="False" type="video">,
 <Stream: itag="136" mime_type="video/mp4" res="720p" fps="30fps" vcodec="avc1.4d401f" progressive="False" type="video">,
 <Stream: itag="247" mime_type="video/webm" res="720p" fps="30fps" vcodec="vp9" progressive="False" type="video">,
 <Stream: itag="398" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.05M.08" progressive="False" type="video">,
 <Stream: itag="135" mime_type="video/mp4" res="480p" fps="30fps" vcodec="avc1.4d401e" progressive="False" type="video">,
 <Stream: itag="244" mime_type="video/webm" res="480p" fps="30fps" vcodec="vp9" progressive="False" type="video">,
 <Stream: itag="397" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.04M.08" progressive="False" type="video">,
 <Stream: itag="134" mime_type="video/mp4" res="360p" fps="30fps" vcodec="avc1.4d401e" progressive="False" type="video">,
 <Stream: itag="243" mime_type="video/webm" res="360p" fps="30fps" vcodec="vp9" progressive="False" type="video">,
 <Stream: itag="396" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.01M.08" progressive="False" type="video">,
 <Stream: itag="133" mime_type="video/mp4" res="240p" fps="30fps" vcodec="avc1.4d4015" progressive="False" type="video">,
 <Stream: itag="242" mime_type="video/webm" res="240p" fps="30fps" vcodec="vp9" progressive="False" type="video">,
 <Stream: itag="395" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.00M.08" progressive="False" type="video">,
 <Stream: itag="160" mime_type="video/mp4" res="144p" fps="30fps" vcodec="avc1.4d400c" progressive="False" type="video">,
 <Stream: itag="278" mime_type="video/webm" res="144p" fps="30fps" vcodec="vp9" progressive="False" type="video">,
 <Stream: itag="394" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.00M.08" progressive="False" type="video">,
 <Stream: itag="140" mime_type="audio/mp4" abr="128kbps" acodec="mp4a.40.2" progressive="False" type="audio">,
 <Stream: itag="249" mime_type="audio/webm" abr="50kbps" acodec="opus" progressive="False" type="audio">,
 <Stream: itag="250" mime_type="audio/webm" abr="70kbps" acodec="opus" progressive="False" type="audio">,
 <Stream: itag="251" mime_type="audio/webm" abr="160kbps" acodec="opus" progressive="False" type="audio">]

You may notice that some streams listed have both a video codec and audio codec, while others have just video or just audio, this is a result of YouTube supporting a streaming technique called Dynamic Adaptive Streaming over HTTP (DASH).

In the context of pytube, the implications are for the highest quality streams; you now need to download both the audio and video tracks and then post-process them with software like FFmpeg to merge them.

The legacy streams that contain the audio and video in a single file (referred to as "progressive download") are still available, but only for resolutions 720p and below.

To only view these progressive download streams:

 >>> yt.streams.filter(progressive=True)
  [<Stream: itag="18" mime_type="video/mp4" res="360p" fps="30fps" vcodec="avc1.42001E" acodec="mp4a.40.2" progressive="True" type="video">,
  <Stream: itag="22" mime_type="video/mp4" res="720p" fps="30fps" vcodec="avc1.64001F" acodec="mp4a.40.2" progressive="True" type="video">]

Conversely, if you only want to see the DASH streams (also referred to as "adaptive") you can do:

>>> yt.streams.filter(adaptive=True)
 [<Stream: itag="137" mime_type="video/mp4" res="1080p" fps="30fps" vcodec="avc1.640028" progressive="False" type="video">,
 <Stream: itag="248" mime_type="video/webm" res="1080p" fps="30fps" vcodec="vp9" progressive="False" type="video">,
 <Stream: itag="399" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.08M.08" progressive="False" type="video">,
 <Stream: itag="136" mime_type="video/mp4" res="720p" fps="30fps" vcodec="avc1.4d401f" progressive="False" type="video">,
 <Stream: itag="247" mime_type="video/webm" res="720p" fps="30fps" vcodec="vp9" progressive="False" type="video">,
 <Stream: itag="398" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.05M.08" progressive="False" type="video">,
 <Stream: itag="135" mime_type="video/mp4" res="480p" fps="30fps" vcodec="avc1.4d401e" progressive="False" type="video">,
 <Stream: itag="244" mime_type="video/webm" res="480p" fps="30fps" vcodec="vp9" progressive="False" type="video">,
 <Stream: itag="397" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.04M.08" progressive="False" type="video">,
 <Stream: itag="134" mime_type="video/mp4" res="360p" fps="30fps" vcodec="avc1.4d401e" progressive="False" type="video">,
 <Stream: itag="243" mime_type="video/webm" res="360p" fps="30fps" vcodec="vp9" progressive="False" type="video">,
 <Stream: itag="396" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.01M.08" progressive="False" type="video">,
 <Stream: itag="133" mime_type="video/mp4" res="240p" fps="30fps" vcodec="avc1.4d4015" progressive="False" type="video">,
 <Stream: itag="242" mime_type="video/webm" res="240p" fps="30fps" vcodec="vp9" progressive="False" type="video">,
 <Stream: itag="395" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.00M.08" progressive="False" type="video">,
 <Stream: itag="160" mime_type="video/mp4" res="144p" fps="30fps" vcodec="avc1.4d400c" progressive="False" type="video">,
 <Stream: itag="278" mime_type="video/webm" res="144p" fps="30fps" vcodec="vp9" progressive="False" type="video">,
 <Stream: itag="394" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.00M.08" progressive="False" type="video">,
 <Stream: itag="140" mime_type="audio/mp4" abr="128kbps" acodec="mp4a.40.2" progressive="False" type="audio">,
 <Stream: itag="249" mime_type="audio/webm" abr="50kbps" acodec="opus" progressive="False" type="audio">,
 <Stream: itag="250" mime_type="audio/webm" abr="70kbps" acodec="opus" progressive="False" type="audio">,
 <Stream: itag="251" mime_type="audio/webm" abr="160kbps" acodec="opus" progressive="False" type="audio">]

You can also interact with Youtube playlists:

>>> from pytube import Playlist
>>> pl = Playlist("https://www.youtube.com/watch?v=Edpy1szoG80&list=PL153hDY-y1E00uQtCVCVC8xJ25TYX8yPU")
>>> for video in pl.videos:
>>>     video.streams.first().download()

Pytube allows you to filter on every property available (see the documentation for the complete list), let's take a look at some of the most useful ones.

To list the audio only streams:

>>> yt.streams.filter(only_audio=True)
  [<Stream: itag="140" mime_type="audio/mp4" abr="128kbps" acodec="mp4a.40.2" progressive="False" type="audio">,
  <Stream: itag="249" mime_type="audio/webm" abr="50kbps" acodec="opus" progressive="False" type="audio">,
  <Stream: itag="250" mime_type="audio/webm" abr="70kbps" acodec="opus" progressive="False" type="audio">,
  <Stream: itag="251" mime_type="audio/webm" abr="160kbps" acodec="opus" progressive="False" type="audio">]

To list only mp4 streams:

>>> yt.streams.filter(subtype='mp4').all()
 [<Stream: itag="18" mime_type="video/mp4" res="360p" fps="30fps" vcodec="avc1.42001E" acodec="mp4a.40.2" progressive="True" type="video">,
 <Stream: itag="22" mime_type="video/mp4" res="720p" fps="30fps" vcodec="avc1.64001F" acodec="mp4a.40.2" progressive="True" type="video">,
 <Stream: itag="137" mime_type="video/mp4" res="1080p" fps="30fps" vcodec="avc1.640028" progressive="False" type="video">,
 <Stream: itag="399" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.08M.08" progressive="False" type="video">,
 <Stream: itag="136" mime_type="video/mp4" res="720p" fps="30fps" vcodec="avc1.4d401f" progressive="False" type="video">,
 <Stream: itag="398" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.05M.08" progressive="False" type="video">,
 <Stream: itag="135" mime_type="video/mp4" res="480p" fps="30fps" vcodec="avc1.4d401e" progressive="False" type="video">,
 <Stream: itag="397" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.04M.08" progressive="False" type="video">,
 <Stream: itag="134" mime_type="video/mp4" res="360p" fps="30fps" vcodec="avc1.4d401e" progressive="False" type="video">,
 <Stream: itag="396" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.01M.08" progressive="False" type="video">,
 <Stream: itag="133" mime_type="video/mp4" res="240p" fps="30fps" vcodec="avc1.4d4015" progressive="False" type="video">,
 <Stream: itag="395" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.00M.08" progressive="False" type="video">,
 <Stream: itag="160" mime_type="video/mp4" res="144p" fps="30fps" vcodec="avc1.4d400c" progressive="False" type="video">,
 <Stream: itag="394" mime_type="video/mp4" res="None" fps="30fps" vcodec="av01.0.00M.08" progressive="False" type="video">,
 <Stream: itag="140" mime_type="audio/mp4" abr="128kbps" acodec="mp4a.40.2" progressive="False" type="audio">]

Multiple filters can also be specified:

>>> yt.streams.filter(subtype='mp4', progressive=True).all()
>>> # this can also be expressed as:
>>> yt.streams.filter(subtype='mp4').filter(progressive=True).all()
  [<Stream: itag="18" mime_type="video/mp4" res="360p" fps="30fps" vcodec="avc1.42001E" acodec="mp4a.40.2" progressive="True" type="video">,
  <Stream: itag="22" mime_type="video/mp4" res="720p" fps="30fps" vcodec="avc1.64001F" acodec="mp4a.40.2" progressive="True" type="video">]

You also have an interface to select streams by their itag, without needing to filter:

>>> yt.streams.get_by_itag(22)
  <Stream: itag="22" mime_type="video/mp4" res="720p" fps="30fps" vcodec="avc1.64001F" acodec="mp4a.40.2" progressive="True" type="video">

If you need to optimize for a specific feature, such as the "highest resolution" or "lowest average bitrate":

>>> yt.streams.filter(progressive=True).order_by('resolution').desc().all()
  [<Stream: itag="22" mime_type="video/mp4" res="720p" fps="30fps" vcodec="avc1.64001F" acodec="mp4a.40.2" progressive="True" type="video">,
  <Stream: itag="18" mime_type="video/mp4" res="360p" fps="30fps" vcodec="avc1.42001E" acodec="mp4a.40.2" progressive="True" type="video">]

Note that order_by cannot be used if your attribute is undefined in any of the Stream instances, so be sure to apply a filter to remove those before calling it.

If your application requires post-processing logic, pytube allows you to specify an "on download complete" callback function:

 >>> def convert_to_aac(stream, file_handle):
         return  # do work

 >>> yt.register_on_complete_callback(convert_to_aac)

Similarly, if your application requires on-download progress logic, pytube exposes a callback for this as well:

 >>> def show_progress_bar(stream, chunk, file_handle, bytes_remaining):
         return  # do work

 >>> yt.register_on_progress_callback(show_progress_bar)

Command-line interface

pytube also ships with a tiny cli interface for downloading and probing videos.

Let's start with downloading:

$ pytube http://youtube.com/watch?v=2lAe1cqCOXo --itag=22

To view available streams:

$ pytube http://youtube.com/watch?v=2lAe1cqCOXo --list

Finally, if you're filing a bug report, the cli contains a link to submit a report
