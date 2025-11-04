<img alt="sonoglyph-logo" src="./assets/images/logo.png" style="margin-left:auto; margin-right:auto; display:block; width:250px;"/>

# SonoGlyph

**SonoGlyph** is a browser-based audio steganography lab that hides images within sound. It embeds visual patterns directly into the spectrogram of an audio file, preserving the original sound while producing faint, ghostlike watermarks visible only in the spectral domain.

## Overview

SonoGlyph converts an input image into a frequency-domain watermark, then reconstructs audio using Griffin–Lim phase recovery and controlled amplitude blending.  
The resulting waveform sounds nearly identical to the source but reveals the embedded image when viewed in a spectrogram.

All processing happens locally using browser-native APIs. No uploads, no network requests, and no external servers — your files remain private and fully under your control.

## Features

- **Spectrogram-domain embedding:** Hides image luminance patterns inside user-defined frequency bands.  
- **Customizable frequency range:** Choose minimum and maximum embedding frequencies.  
- **Griffin–Lim reconstruction:** Iterative phase estimation produces high-fidelity, phase-consistent results.  
- **Bandpass filtering:** Linear-phase FIR filters confine the watermark to the selected range.  
- **Visual spectrogram preview:** Compare before-and-after audio spectrograms directly in the browser.  
- **Adjustable parameters:** Control FFT size, hop length, watermark strength, and iteration count.  
- **Offline operation:** Everything runs locally via Canvas and Web Audio APIs.  
- **Lossless export:** Encodes output as 16-bit PCM WAV for precise round-trip consistency.

## Technical Notes

### Processing Pipeline

1. **Audio decoding**  
   Loads and decodes the source audio using the Web Audio API, downmixing to mono if needed.

2. **Spectrogram analysis**  
   Performs Short-Time Fourier Transform (STFT) using a Hann window to generate time-frequency magnitude data.

3. **Image embedding**  
   Converts the uploaded image into normalized luminance values and maps them to target frequency bins between `minHz` and `maxHz`.  
   Image inversion and vertical flipping are supported for control over embedding orientation.

4. **Phase reconstruction**  
   Uses the Griffin–Lim algorithm with user-defined iteration count to generate a coherent phase field.

5. **Filtering and mixing**  
   Applies both STFT-domain and linear-phase time-domain bandpass filters to isolate the watermark.  
   The watermark’s RMS level is scaled relative to the source signal according to the selected mix strength.

6. **Rendering and export**  
   Reconstructs audio with an inverse STFT, normalizes peak amplitude, and encodes the result to a downloadable WAV file.

### Spectrogram Rendering

- Magnitude values are logarithmically scaled and color-mapped using a built-in lookup table (blue-to-amber gradient).  
- The original and watermarked spectrograms are rendered side-by-side on HTML5 `<canvas>` elements for visual comparison.  
- All drawing uses pixelated scaling to preserve detail.

## Intended Users

SonoGlyph was designed for artists, puzzle creators, and ARG designers who explore sound as a carrier of hidden information.  
It is also useful for researchers studying steganographic audio techniques or phase reconstruction algorithms.  

The tool provides a complete, transparent environment for experimentation, revealing the threshold where data becomes art — and where the inaudible becomes visible.

## Usage

1. Open the app in your browser.  
2. Load an **audio file** and an **image** to embed.  
3. Adjust FFT size, hop percentage, and frequency range.  
4. Select watermark strength and Griffin–Lim iteration count.  
5. Preview spectrograms or click **Embed Image & Download WAV** to generate the stego audio.  
6. Use any spectrogram viewer (or ReSounder) to visualize the hidden image.

## License

MIT License — free for modification and use. Attribution appreciated if used publicly.

### Third-Party Components

SonoGlyph includes a local copy of [Bootstrap](https://getbootstrap.com),  
© Twitter, Inc. and contributors, distributed under the MIT License.  
No other third-party libraries or frameworks are used.

## Credit

Created by **NQR** for artists, ARG designers, and curious minds exploring the hidden language of sound.  
If you use *SonoGlyph* in a project or performance, I’d love to hear about it.
