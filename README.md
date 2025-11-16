<img alt="sonoglyph-logo" src="./assets/images/logo.png" style="margin-left:auto; margin-right:auto; display:block; width:250px;"/>

# SonoGlyph

**SonoGlyph** is an interactive browser-based audio steganography tool that hides images within sound. It features a **fully interactive spectrogram interface** with visual controls for precise placement of hidden messages. Embed visual patterns directly into the spectrogram of an audio file, preserving the original sound while producing ghostlike watermarks visible only in the spectral domain.

[**View Live Demo**](https://nqrlabs.com/SonoGlyph/)

## 🎯 Key Interactive Features

- **2D Selection Box** – Drag and resize a cyan rectangle directly on the spectrogram to control exactly *when* (time) and *where* (frequency) your hidden image appears
- **Real-time Playback Indicator** – Green vertical line tracks audio playback position on the spectrogram; drag it to seek instantly
- **Visual Feedback** – See your embedding region update in real-time as you adjust parameters
- **No Guesswork** – Precise visual control eliminates trial-and-error workflow

## Overview

SonoGlyph converts an input image into a frequency-domain watermark, then reconstructs audio using Griffin–Lim phase recovery and controlled amplitude blending. The resulting waveform sounds nearly identical to the source but reveals the embedded image when viewed in a spectrogram.

**What makes SonoGlyph unique?** The **interactive spectrogram interface** lets you visually control exactly where and when your hidden message appears — no trial-and-error, no guesswork. Just drag the cyan selection box to define your embedding region, watch the green playback indicator track audio position in real-time, and export when satisfied.

All processing happens locally using browser-native APIs. No uploads, no network requests, and no external servers — your files remain private and fully under your control.

## Features

### 🎨 Interactive Spectrogram Interface

- **2D Selection Box** – Drag, resize, and reposition a cyan rectangle directly on the spectrogram
  - Left/right edges control **time range** (when the image appears)
  - Top/bottom edges control **frequency range** (where in the spectrum it hides)
  - Drag corners to adjust both dimensions simultaneously
  - Drag center to move entire selection without resizing
  - Click and drag outside to create new selection region
- **Real-time Playback Indicator** – Animated green vertical line shows current audio position
  - Tracks playback automatically as audio plays
  - Drag the indicator to seek/scrub to any timestamp
  - Click anywhere on the spectrogram to jump to that moment
- **Visual Feedback** – All parameters update the display in real-time as you adjust them

### 🔊 Audio Steganography Engine

- **Spectrogram-domain embedding** – Hides image luminance patterns inside user-defined frequency bands
- **Time-windowed embedding** – Precise control over exactly when the hidden image appears
- **Griffin–Lim reconstruction** – Iterative phase estimation produces high-fidelity, phase-consistent results
- **Bandpass filtering** – Linear-phase FIR filters confine the watermark to the selected range
- **Adjustable parameters** – Control FFT size, hop length, watermark strength, and iteration count
- **Offline operation** – Everything runs locally via Canvas and Web Audio APIs
- **Lossless export** – Encodes output as 16-bit PCM WAV for precise round-trip consistency

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

SonoGlyph was designed for **artists, puzzle creators, and ARG designers** who explore sound as a carrier of hidden information. The interactive spectrogram interface makes it ideal for:

- **ARG/Puzzle Creators** – Precisely position clues at specific timestamps and frequencies for timed reveals
- **Audio Artists** – Experiment with the intersection of sound and visual steganography
- **Game Designers** – Create multi-layered audio puzzles with visual verification
- **Security Researchers** – Study steganographic audio techniques and phase reconstruction algorithms

The **visual selection tools** eliminate guesswork and enable rapid iteration — see exactly where your hidden message will appear before embedding. Perfect for creating complex multi-stage puzzles where precise control matters.

## Usage

### Interactive Workflow

1. **Load Your Files**
   - Choose an audio file (WAV, MP3, OGG, etc.)
   - Select an image to embed (high-contrast images work best)

2. **Preview the Spectrogram**
   - Click **"Preview spectrogram"** to visualize your audio's frequency content
   - The cyan selection box appears, showing the default embedding region

3. **Position Your Hidden Message** *(This is where the magic happens!)*
   - **Drag the cyan box** to reposition where/when the image appears
   - **Drag edges** to adjust time range (horizontal) or frequency range (vertical)
   - **Drag corners** to adjust both dimensions at once
   - **Click and drag outside** the box to create an entirely new selection region
   - Watch the input fields update in real-time as you adjust

4. **Fine-Tune Parameters**
   - Adjust **FFT size** for frequency resolution (higher = sharper)
   - Set **Hop %** for time resolution (lower = smoother)
   - Adjust **Mix strength** to control visibility vs. audibility
   - Set **Griffin–Lim iterations** for phase reconstruction quality (6-10 recommended)

5. **Embed and Verify**
   - Click **"Embed image"** to process the steganography
   - Preview updates automatically showing the modified spectrogram
   - Use the **green playback indicator** to verify placement:
     - Watch it move as the audio plays
     - Drag it to scrub through the audio
     - Click anywhere on the spectrogram to jump to that timestamp

6. **Download Your Stego Audio**
   - Click **"Download WAV"** to save the file
   - Share it with players or test it in spectrogram viewers (Audacity, Sonic Visualiser, SpectroGhost, ReSounder)

## Why Interactive Controls Matter for ARG Design

Traditional audio steganography tools require manual entry of time ranges and frequency bands, leading to trial-and-error workflows. SonoGlyph's **visual selection interface** changes this:

### Precise Timed Reveals
- See exactly when your hidden message appears in the audio timeline
- Create multi-stage puzzles where clues appear at specific story moments
- No more guessing whether "2.5 to 4.8 seconds" is the right range

### Frequency Layer Control
- Visually separate clues into different frequency bands
- High frequencies (16-20 kHz) for subtle clues, low frequencies (0-4 kHz) for obvious ones
- Stack multiple messages without overlap — see the boundaries clearly

### Rapid Iteration
- Drag, test, adjust, repeat — iterate in seconds instead of minutes
- Real-time playback verification shows exactly where players will find clues
- Export only when you're satisfied with the placement

### Coordinate-Based Puzzles
- Share precise time/frequency coordinates with players
- "Look at 12-18 kHz between 30-45 seconds" becomes a visual target they can verify
- Create puzzles that require finding multiple specific regions

## License

MIT License — free for modification and use. Attribution appreciated if used publicly.

## Credit

Created by **NQR** for artists, ARG designers, and curious minds exploring the hidden language of sound.  
If you use *SonoGlyph* in a project or performance, I’d love to hear about it.
