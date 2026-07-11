---
layout: default
---

# Editing raw sound with ffmpeg

After downloading ffmpeg binary, following command can be used to play raw sound bytes.

~~~~~~~~~~~~~ shell
// ffmpeg play raw sound bytes
ffplay -f s16le -ar 44100 -i input.raw -ch_layout stereo
~~~~~~~~~~~~~

Parameters:
* -f s16le: Sets the input format. s16le stands for Signed 16-bit Little Endian, which is the most common raw audio format.
* -ar 44100: Sets the audio sample rate to 44,100 Hz (CD quality).
* -ch_layout stereo: Sets the number of audio channels to 2 (stereo).
* -i input.raw: Specifies your raw data file or input source.


~~~~~~~~~~~~~ shell
// ffmpeg wav file extract raw bytes lower quality
ffmpeg -i input.wav -f s16le -ar 22050 -ac 1 -c:a pcm_s16le output_raw.bin
~~~~~~~~~~~~~

Parameters:
* -i input.wav: Specifies your original audio file.
* -f s16le: Forces the format to raw 16-bit little-endian PCM. This strips the WAV container, leaving only the raw audio bytes.
* -ar 22050: Drops the sample rate to 22,050 Hz (down from the standard 44,100 Hz to lower the quality and file size.
* -ac 1: Converts the audio to mono.
* output_raw.bin: The output file containing the raw bytes.



~~~~~~~~~~~~~ shell
// extract raw bytes from wav fle
ffmpeg -i input.wav -f s16le -acodec pcm_s16le output.raw
~~~~~~~~~~~~~

Parameters:
* -f s16le: Forces the output format to raw signed 16-bit little-endian.
* -acodec pcm_s16le: Configures the codec to uncompressed PCM audio.





~~~~~~~~~~~~~ shell
// show wav file properties
ffprobe -v error -select_streams a -show_entries stream input.wav
~~~~~~~~~~~~~



~~~~~~~~~~~~~ shell
// To convert a signed 16-bit WAV file into unsigned 8-bit raw bytes (headerless PCM data) using FFmpeg,
ffmpeg -i input.wav -f u8 -c:a pcm_u8 output.raw
~~~~~~~~~~~~~

Parameters:
* -i input.wav: Specifies your source WAV file.
* -f u8: Forces the output container format to be raw unsigned 8-bit data (stripping the WAV header entirely).
-c:a pcm_u8: Forces the audio codec to convert the audio samples into an unsigned 8-bit PCM format.
output.raw: The resulting file containing only the raw, uncompressed byte stream.
