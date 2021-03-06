---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-07"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Audio formats
{: #audio-formats}

The {{site.data.keyword.speechtotextfull}} service can extract speech from audio in many different formats.

-   If you are unfamiliar with audio and how it is described and specified, begin with [Audio characteristics and terminology](#terminology) to help you get started.
-   If you already understand how to use audio, jump to [Supported audio formats](#formats) for detailed information about the formats that the service supports.

The final sections, [Data limits and compression](#limits) and [Audio conversion](#conversion), can help you get the most from your use of the service.

How you record audio can make a big difference in the service's results. Speech recognition can be very sensitive to input audio quality. When you experiment with the service, try to ensure that the input audio quality is as good as possible. To obtain the best possible accuracy, use a close, speech-oriented microphone (such as a headset) whenever possible and adjust the microphone settings if necessary. Try to avoid using a system's built-in microphone.
{: tip}

## Audio characteristics and terminology
{: #terminology}

The following terminology is used to describe the characteristics of audio data for the different formats.

### Sampling rate
{: #samplingRate}

*Sampling rate* (or sampling frequency) is the number of audio samples that are taken per second. Sampling rate is measured in Hertz (Hz) or kilohertz (kHz). For example, a rate of 16,000 samples per second is equal to 16,000 Hz (or 16 kHz). With the {{site.data.keyword.speechtotextshort}} service, you specify a model to indicate the sampling rate of your audio:

-   *Broadband* models are used for audio that is sampled at no less than 16 kHz, which {{site.data.keyword.IBM}} recommends for responsive, real-time applications (for example, for live-speech applications).
-   *Narrowband* models are used for audio that is sampled at no less than 8 kHz, which is the rate that is typically used for telephonic audio.

The service supports both broadband and narrowband audio for most languages and formats. It automatically adjusts the sampling rate of your audio to match the model that you specify before it recognizes speech.

-   For broadband models, the service converts audio recorded at higher sampling rates to 16 kHz.
-   For narrowband models, it converts the audio to 8 kHz.

In theory, you can send 44 kHz audio with a broadband or narrowband model, but that needlessly increases the size of the audio. To maximize the amount of audio that you can send, match the sampling rate of your audio to the model that you use. The service does not accept audio that is sampled at a rate that is less than the intrinsic sampling rate of the model.

**Notes about audio formats**

-   For the `audio/l16` and `audio/mulaw` formats, you must specify the rate of your audio.
-   For the `audio/basic` and `audio/g729` formats, the service supports only narrowband audio.

**More information**

-   For more information about sampling rates, see [en.wikipedia.org/wiki/Sampling ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Sampling){: new_window}. Select *Sampling (signal processing)*.
-   For more information about the models that the service offers for each supported language, see [Languages and models](/docs/services/speech-to-text/models.html).

### Bit rate
{: #bitRate}

*Bit rate* is the number of bits of data that is sent per second. The bit rate for an audio stream is measured in kilobits per second (kbps). The bit rate is calculated from the sampling rate and the number of bits stored per sample. For speech recognition, {{site.data.keyword.IBM}} recommends that you record 16 bits per sample for your audio.

For example, audio that uses a broadband sampling rate of 16 kHz and 16 bits per sample has a bit rate of 256 kbps: `(16,000 * 16) / 1000`.

**More information**

-   For more information about bit rates, see [en.wikipedia.org/wiki/Bit_rate ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Bit_rate){: new_window}.
-   For a general discussion of sampling rates and bit rates, see [What are bit rates? ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.richardfarrar.com/what-are-bit-rates/){: new_window} and [Choosing bit rates for podcasts ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.richardfarrar.com/choosing-bit-rates-for-podcasts/){: new_window}.

### Compression
{: #compression}

*Compression* is used by many audio formats to reduce the size of the audio data. Compression reduces the number of bits stored per sample and thus the bit rate. Some formats use no compression, but most use one of the basic types of compression:

-   *Lossless* compression reduces the size of the audio with no loss of quality, but the compression ratio is typically small.
-   *Lossy* compression reduces the size of the audio by as much as 10 times, but some data and some quality is irretrievably lost in the compression.

With the {{site.data.keyword.speechtotextshort}} service, you can safely use lossy compression to maximize the amount of audio that you can send to the service with a recognition request. Because the dynamic range of the human voice is more limited than, say, music, speech can accommodate a bit rate that is much lower than other types of audio. For speech recognition, {{site.data.keyword.IBM}} recommends that you use 16 bits per sample for your audio and employ a format that compresses the audio data.

**Notes about audio formats**

-   The `audio/ogg` and `audio/webm` formats are containers whose compression relies on the codec that you use to encode the data: Opus or Vorbis.
-   The `audio/wav` format is a container that can include uncompressed, lossless, or lossy data.

**More information**

-   For more information about audio compression, see [en.wikipedia.org/wiki/Data_compression#Audio ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Data_compression#Audio){: new_window}.
-   For more information about using data compression to increase the amount of audio that you can send with a request, see [Data limits and compression](#limits).

### Channels
{: #channels}

*Channels* indicate the number of streams of the recorded audio:

-   *Monaural* (or mono) audio has only a single channel.
-   *Stereophonic* (or stereo) audio typically has two channels.

The {{site.data.keyword.speechtotextshort}} service accepts audio with a maximum of 16 channels. Because it uses only a single channel for speech recognition, the service downmixes audio with multiple channels to one-channel mono during transcoding.

**Notes about audio formats**

-   For the `audio/l16` format, you must specify the number of channels if your audio has more than one channel.
-   For the `audio/wav` format, the service accepts audio with a maximum of nine channels.

**More information**

-   For more information about audio channels, see [en.wikipedia.org/wiki/Audio_signal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Audio_signal){: new_window}.

### Endianness
{: #endianness}

*Endianness* indicates how bytes of data are organized by the underlying computer architecture:

-   *Big-endian* orders data by most-significant bit.
-   *Little-endian* orders data by least-significant bit.

The {{site.data.keyword.speechtotextshort}} service automatically detects the endianness of the incoming audio.

**Notes about audio formats**

-   For the `audio/l16` format, you can specify the endianness to disable auto-detection if needed.

**More information**

-   For more information about endianness, see [en.wikipedia.org/wiki/Endianness ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Endianness){: new_window}.

## Supported audio formats
{: #formats}

Table 1 provides a summary of the audio formats that the service supports.

-   *Audio format and compression* identifies each format and indicates its supported compression. You can send a maximum of 100 MB of audio to the service with a single request. By using a format that supports compression, you can reduce the size of your audio to maximize the amount of data that you can pass to the service. For more information, see [Data limits and compression](#limits).
-   *Content-type specification* indicates whether you must use the `Content-Type` header or equivalent parameter to specify the format (MIME type) of the audio that you send to the service. You can specify the audio format for any request, but that's not always necessary:
    -   For most formats, the content type is optional. You can omit the content type or specify `application/octet-stream` to have the service automatically detect the format.
    -   For others, the content type is required. These formats do not provide the information, such as the sampling rate, that the service needs to auto-detect their format.
-   The final columns identify additional *Required parameters* and *Optional parameters* for each format. The following sections provide more information about these parameters.

<table>
  <caption>Table 1. Summary of supported audio formats</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom">
      Audio format<br>and compression
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Content-type<br/>specification
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Required<br/>parameters
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Optional<br/>parameters
    </th>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/basic](#basic)<br/>Lossy
    </td>
    <td style="text-align:center">
      Required
    </td>
    <td style="text-align:center">
      None
    </td>
    <td style="text-align:center">
      None
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/flac](#flac)<br/>Lossless
    </td>
    <td style="text-align:center">
      Optional
    </td>
    <td style="text-align:center">
      None
    </td>
    <td style="text-align:center">
      None
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/g729](#g729)<br/>Lossy
    </td>
    <td style="text-align:center">
      Optional
    </td>
    <td style="text-align:center">
      None
    </td>
    <td style="text-align:center">
      None
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/l16](#l16)<br/>None
    </td>
    <td style="text-align:center">
      Required
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      <code>channels={integer}</code><br/>
      <code>endianness=big-endian</code><br/>
      <code>endianness=little-endian</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mp3](#mp3)<br/>
      [audio/mpeg](#mp3)<br/>Lossy
    </td>
    <td style="text-align:center">
      Optional
    </td>
    <td style="text-align:center">
      None
    </td>
    <td style="text-align:center">
      None
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mulaw](#mulaw)<br/>Lossy
    </td>
    <td style="text-align:center">
      Required
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      None
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/ogg](#ogg)<br/>Lossy
    </td>
    <td style="text-align:center">
      Optional
    </td>
    <td style="text-align:center">
      None
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/wav](#wav)<br/>None, lossless,<br/>or lossy
    </td>
    <td style="text-align:center">
      Optional
    </td>
    <td style="text-align:center">
      None
    </td>
    <td style="text-align:center">
      None
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/webm](#webm)<br/>Lossy
    </td>
    <td style="text-align:center">
      Optional
    </td>
    <td style="text-align:center">
      None
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
</table>

When you use the `curl` command to make a speech recognition request with the HTTP interfaces, you must either specify the audio format with the `Content-Type` header, specify `"Content-Type: application/octet-stream"`, or specify `"Content-Type:"`. If you omit the header entirely, `curl` uses a default value of `application/x-www-form-urlencoded`.
{: important}

### audio/basic format
{: #basic}

*Basic audio* (`audio/basic`) is a single-channel, lossy audio format that is encoded by using 8-bit u-law (or mu-law) data that is sampled at 8 kHz. This format provides a lowest-common denominator for indicating the media type of audio. The service supports the use of files in `audio/basic` format only with narrowband models.

For more information, see the Internet Engineering Task Force (IETF) [Request for Comment (RFC) 2046 ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://tools.ietf.org/html/rfc2046){: new_window} and [iana.org/assignments/media-types/audio/basic ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.iana.org/assignments/media-types/audio/basic){: new_window}.

### audio/flac format
{: #flac}

*Free Lossless Audio Codec (FLAC)* (`audio/flac`) is a lossless audio format. For more information, see [en.wikipedia.org/wiki/FLAC ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/FLAC){: new_window}.

### audio/g729 format
{: #g729}

*G.729* (`audio/g729`) is a lossy audio format that supports data that is encoded at 8 kHz. The service supports only G.729 Annex D, not Annex J. The service supports the use of files in `audio/g729` format only with narrowband models. For more information, see [en.wikipedia.org/wiki/G.729 ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/G.729){: new_window}.

### audio/l16 format
{: #l16}

*Linear 16-bit Pulse-Code Modulation (PCM)* (`audio/l16`) is an uncompressed audio format. Use this format to pass a raw PCM file. Linear PCM audio can also be carried inside of a container Waveform Audio File Format (WAV) file. When you use the `audio/l16` format, the service accepts extra required and optional parameters on the format specification.

<table>
  <caption>Table 2. Parameters for `audio/l16` format</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Parameter
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Description
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Required</em>
    </td>
    <td>
      An integer that specifies the sampling rate at which the audio is
      captured. For example, specify the following parameter for audio
      data that is captured at 16 kHz:<br/><br/>
      <code>audio/l16;rate=16000</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>channels</code><br/><em>Optional</em>
    </td>
    <td>
      By default, the service treats the audio as if it has a single channel.
      <em>If the audio has more than one channel,</em> you must specify
      an integer that identifies the number of channels. For example,
      specify the following parameter for two-channel audio data that is
      captured at 16 kHz:<br/><br/>
      <code>audio/l16;rate=16000;channels=2</code><br/><br/>
      The service accepts a maximum of 16 channels. It downmixes the audio
      to one channel during transcoding.
    </td>
  </tr>
  <tr>
    <td>
      <code>endianness</code><br/><em>Optional</em>
    </td>
    <td>
      By default, the service auto-detects the endianness of incoming
      audio. But its auto-detection can sometimes fail and drop the
      connection for short audio in `audio/l16` format. Specifying
      the endianness disables auto-detection. Specify either
      <code>big-endian</code> or <code>little-endian</code>. For
      example, specify the following parameter for audio data that
      is captured at 16 kHz in little-endian format:<br/><br/>
      <code>audio/l16;rate=16000;endianness=little-endian</code><br/><br/>
      Section 5.1 of
      <a target="_blank" href="https://tools.ietf.org/html/rfc2045#section-5.1">Request for Comment (RFC) 2045 ![External link icon](../../icons/launch-glyph.svg "External link icon")</a>
      specifies big-endian format for <code>audio/l16</code> data, but
      many people use little-endian format.
    </td>
  </tr>
</table>

For more information, see the IETF [Request for Comment (RFC) 2586 ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://tools.ietf.org/html/rfc2586){: new_window} and [en.wikipedia.org/wiki/Pulse-code_modulation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Pulse-code_modulation){: new_window}.

### audio/mp3 and audio/mpeg formats
{: #mp3}

*MP3* (`audio/mp3`) or *Motion Picture Experts Group (MPEG)* (`audio/mpeg`) is a lossy audio format. (MP3 and MPEG refer to the same format.) For more information, see [en.wikipedia.org/wiki/MP3 ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/MP3){: new_window}.

### audio/mulaw format
{: #mulaw}

*Mu-law* (`audio/mulaw`) is a single-channel, lossy audio format. The data is encoded by using the u-law (or mu-law) algorithm. When you use this format, the service requires an extra parameter on the format specification.

<table>
  <caption>Table 3. Parameter for `audio/mulaw` format</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Parameter
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Description
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Required</em>
    </td>
    <td>
      An integer that specifies the sampling rate at which the audio is
      captured. For example, specify the following parameter for audio
      data that is captured at 8 kHz:<br/><br/>
      <code>audio/mulaw;rate=8000</code>
    </td>
  </tr>
</table>

For more information, see [en.wikipedia.org/wiki/M-law_algorithm ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/M-law_algorithm){: new_window}.

### audio/ogg format
{: #ogg}

*Ogg* (`audio/ogg`) is an open container format that is maintained by the Xiph.org Foundation ([xiph.org/ogg ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.xiph.org/ogg){: new_window}). You can use audio streams that are compressed with the following lossy codecs:

-   *Opus* (`audio/ogg;codecs=opus`). For more information, see [opus-codec.org ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.opus-codec.org/){: new_window} and [en.wikipedia.org/wiki/Opus (audio format) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Opus){: new_window}. On the *Opus (audio format)* page, look especially at the *Containers* section.
-   *Vorbis* (`audio/ogg;codecs=vorbis`). For more information, see [xiph.org/vorbis ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://xiph.org/vorbis/){: new_window} and [en.wikipedia.org/wiki/Vorbis ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Vorbis){: new_window}.

If you omit the codec, the service automatically detects it from the input audio. Opus is the preferred codec; it is standardized by the Internet Engineering Task Force (IETF) as [Request for Comment (RFC) 6716 ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://tools.ietf.org/html/rfc6716){: new_window}.

### audio/wav format
{: #wav}

*Waveform Audio File Format (WAV)* (`audio/wav`) is a container format that is often used for uncompressed audio streams, but it can contain compressed audio, as well. The service supports WAV audio that uses any encoding. It accepts WAV audio with a maximum of nine channels (due to an FFmpeg limitation).

For more information about the WAV format, see [en.wikipedia.org/wiki/WAV ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/WAV){: new_window}. For more information about reducing the size of WAV audio by converting it to the Opus codec, see [Converting to audio/ogg with the Opus codec](#conversionOgg).

### audio/webm format
{: #webm}

*Web Media (WebM)* (`audio/webm`) is an open container format that is maintained by the WebM project ([webmproject.org ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.webmproject.org/){: new_window}). You can use audio streams that are compressed with the following lossy codecs:

-   *Opus* (`audio/webm;codecs=opus`). For more information, see [opus-codec.org ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.opus-codec.org/){: new_window} and [en.wikipedia.org/wiki/Opus (audio format) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Opus){: new_window}. On the *Opus (audio format)* page, look especially at the *Containers* section.
-   *Vorbis* (`audio/webm;codecs=vorbis`). For more information, see [xiph.org/vorbis ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://xiph.org/vorbis/){: new_window} and [en.wikipedia.org/wiki/Vorbis ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Vorbis){: new_window}.

If you omit the codec, the service automatically detects it from the input audio.

For JavaScript code that shows how to capture audio from a microphone in a Chrome browser and encode it into a WebM data stream, see [jsbin.com/hedujihuqo/edit?js,console ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://jsbin.com/hedujihuqo/edit?js,console){: new_window}. The code does not submit the captured audio to the service.
{: tip}

## Data limits and compression
{: #limits}

The service accepts a maximum of 100 MB of audio data for transcription with a request. When you recognize long continuous audio streams or large files, take the following steps to ensure that your audio does not exceed the 100 MB limit:

-   Use a sampling rate no greater than 16 kHz (for broadband models) or 8 kHz (for narrowband models), and use 16 bits per sample. The service converts audio recorded at a sampling rate that is higher than the target model (16 kHz or 8 kHz) to the rate of the model. So larger frequencies do not result in enhanced recognition accuracy, but they do increase the size of the audio stream.
-   Encode your audio in a format that offers data compression. By encoding your data more efficiently, you can send far more audio without exceeding the 100 MB data limit. Audio formats such as `audio/ogg` and `audio/mp3` significantly reduce the size of your audio stream. You can use these formats to send greater amounts of audio with a single request.

If you compress the audio too severely with the `audio/ogg` format, you can lose some recognition accuracy. To be safe, retain a bit rate of at least 24 kbps for your audio.

### Comparing approximate audio sizes
{: #compareSizes}

Consider the approximate size of the data stream that results from 2 hours of continuous speech transmission that is sampled at 16 kHz and at 16 bits per sample:

-   If the data is encoded with the `audio/wav` format, the two-hour stream has a size of 230 MB, well beyond the service's 100 MB limit.
-   If the data is encoded in the `audio/ogg` format, the two-hour stream has a size of only 23 MB, well beneath the service's limit.

The following table approximates the maximum duration of audio that can be sent with a recognition request in different formats given the 100 MB service limit. Actual values can vary depending on the complexity of the audio and the achieved compression rate.

<table style="width:75%">
  <caption>Table 4. Maximum duration of audio in different formats</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 50%">
      Audio format
    </th>
    <th style="text-align:center; vertical-align:bottom; width 50%">
      Maximum duration of audio (approximate)
    </th>
  </tr>
  <tr>
    <td><code>audio/wav</code></td>
    <td style="text-align:center">55 minutes</td>
  </tr>
  <tr>
    <td><code>audio/flac</code></td>
    <td style="text-align:center">1 hour 40 minutes</td>
  </tr>
  <tr>
    <td><code>audio/mp3</code></td>
    <td style="text-align:center">3 hours 20 minutes</td>
  </tr>
  <tr>
    <td><code>audio/ogg</code></td>
    <td style="text-align:center">8 hours 40 minutes</td>
  </tr>
</table>

The `audio/ogg;codecs=opus` and `audio/webm;codecs=opus` formats are generally equivalent, and their sizes are almost identical. They use the same codec internally; only the container format is different.

## Audio conversion
{: #conversion}

You can use various tools to convert your audio to a different format. The tools can be helpful when your audio is in a format that is not supported by the service or is in an uncompressed or lossless format. In the latter case, you can convert the audio to a lossy format to reduce its size.

The following freeware tools are available to convert your audio from one format to another:

-   Sound eXchange (SoX) ([sox.sourceforge.net ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://sox.sourceforge.net){: new_window})
-   FFmpeg ([ffmpeg.org ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ffmpeg.org){: new_window})
-   Audacity&reg; ([audacityteam.org ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.audacityteam.org/){: new_window})
-   For Ogg format with the Opus codec, **opus-tools** ([opus-codec.org/downloads/ ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://opus-codec.org/downloads/){: new_window})

These tools offer cross-platform support for multiple audio formats. Moreover, you can use many of the tools to play your audio. Do not use the tools to violate applicable copyright laws.

### Converting to audio/ogg with the Opus codec
{: #conversionOgg}

The [**opus-tools** ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://opus-codec.org/downloads/){: new_window} include three command-line utilities for working with Ogg audio in the Opus codec:

-   The [**opusenc** ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusenc.html){: new_window} utility encodes audio from WAV, FLAC, and other formats to Ogg with the Opus codec. The page shows how to compress audio streams. Compression is useful for passing real-time audio to the service.
-   The [**opusdec** ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusdec.html){: new_window} utility decodes audio from the Opus codec to uncompressed PCM WAV files.
-   The [**opusinfo** ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusinfo.html){: new_window} utility provides information about and validity checking for Opus files.

Many users send WAV files for speech recognition. Given the service's 100 MB data limit, WAV format reduces the amount of audio that can be recognized with a single request. Using the **opusenc** command to convert the audio to the preferred `audio/ogg:codecs=opus` format can greatly increase the amount of audio that you can send with a recognition request.

For example, consider an uncompressed broadband (16 kHz) WAV file (**input.wav**) that uses 16 bits per sample for a bit rate of 256 kbps. The following command converts the audio to a file (**output.opus**) that uses the Opus codec:

```bash
opusenc input.wav output.opus
```
{: pre}

The conversion compresses the audio by a factor of four and produces an output file with a bit rate of 64 kbps. However, according to the [Opus Recommended Settings ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://wiki.xiph.org/Opus_Recommended_Settings){: new_window}, you can safely reduce the bit rate to 24 kbps and still retain a full band for speech audio. This reduction compresses the audio by a factor of 10. The following command uses the `--bitrate` option to produce an output file with a bit rate of 24 kbps:

```bash
opusenc --bitrate 24 input.wav output.opus
```
{: pre}

Compression with the **opusenc** utility is fast. Compression happens at a rate that is approximately 100 times faster than real time. When it finishes, the command writes output to the console that provides complete details about its running time and the resulting audio data.
