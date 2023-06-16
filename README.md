# uxnwave

WAV audio player for [Uxn](https://100r.co/site/uxn.html).

uxnwave can play files much larger than Uxn's 64k address space, and can output "high-quality" 12-bit PCM audio using some trickery.

This is only a proof of concept, eventually I will add playback controls and a GUI.

## Usage

Download the ROM [here](https://github.com/vanjac/uxnwave/releases/latest/download/wavplay.rom), or build using uxnasm. drifblim will not work since it lacks macro support.

You'll need a Uxn emulator. [Uxn](https://100r.co/site/uxn.html) and [Uxn32](https://github.com/randrew/uxn32) have been tested, anything else may not work correctly.

You'll also need a WAV file to play. WAVs must be **16-bit stereo, 44100Hz**. Anything else will not play back correctly.

Pass the WAV file to be played as a command line argument:

`uxn wavplay.rom file.wav`

Press Space to pause/resume playback.

## How it works

All 4 audio devices are playing a looping sample. Audio is streamed into each buffer as it is playing -- the buffers are split in half, and while one half plays, the other half is loading audio data from the file.

Varvara audio devices can only play back 8-bit audio. To get around this, uxnwave uses *two* audio devices per stereo channel. One device plays the upper 8 bits of each 12-bit PCM sample, and the other plays the lower 4 bits at 1/16th volume. When mixed together, you hear the full 12 bits of audio.
