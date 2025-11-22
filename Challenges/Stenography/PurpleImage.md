# PurpleImage

There are several indicators that can reveal unusual characteristics in an audio file:
- The file size: An atypically large or small WAV file may suggest hidden data or unconventional encoding.
- The sound itself: If, when listening, the audio consists only of monotonic vibrations or unnatural tones, it may indicate that the signal is structured for analysis rather than for normal playback.

```bash
sox challenge_morse.wav -n spectrogram -o spectrogram.png
eog spectrogram.png
```
