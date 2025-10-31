# Deobfuscation
Editing source code and transforming its presentation can increase the effort required for unauthorized reuse. 

- Code in one line: [Prettier Playground] (https://prettier.io/playground)
- Encoded code: [UnPacker] (https://matthewfl.com/unPacker.html)

# Decode
## Base64
- Base64 Encode: echo https://www.hackthebox.eu/ | base64
- Base64 Decode: echo https://www.hackthebox.eu/ | base64 -d
## Hex
- Hex ENdonde: echo https://www.hackthebox.eu/ | xxd -p
- Hex Decode: echo 68747470733a2f2f7777772e6861636b746865626f782e65752f0a | xxd -p -r
## Caesar/Rot13
- Rot13 Encode: echo https://www.hackthebox.eu/ | tr 'A-Za-z' 'N-ZA-Mn-za-m'
- Rot13 Decode: echo uggcf://jjj.unpxgurobk.rh/ | tr 'A-Za-z' 'N-ZA-Mn-za-m'
