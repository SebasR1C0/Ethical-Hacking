# Morse

- Increase audio
```bash
sox challenge_morse.wav boosted.wav gain 10
```
- Reverse audio
```bash
sox challenge_morse.wav reversed.wav reverse
```
- To text
```bash
multimon-ng -t wav -a MORSE_CW boosted.wav
```
