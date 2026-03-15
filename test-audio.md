# PeaceMaker Audio System Test Checklist

## Pre-Test Setup
- Open: http://localhost:8888/index.html
- Open browser console (F12 / Cmd+Option+I)
- Ensure speakers/headphones are connected and unmuted
- Volume should be at comfortable level

## Test 1: Initial Load
✓ Audio controls panel appears in left sidebar with:
  - [ ] Voice checkbox (checked by default)
  - [ ] Ambience checkbox (checked by default)
  - [ ] Sounds checkbox (checked by default)
  - [ ] Volume slider (set to 60%)

## Test 2: Session Start
When clicking "Begin Session":
  - [ ] Welcome message: "Welcome. Take a deep breath. Let the shapes guide your focus." (spoken after 1 second)
  - [ ] Background ambience starts (low drone + soft noise)
  - [ ] First breathing instruction begins

## Test 3: Voice Coaching - Breathing
Listen for voice speaking in cycle (every 9.5 seconds):
  - [ ] "Inhale slowly" (at 0s)
  - [ ] "Hold gently" (at 3s)
  - [ ] "Exhale softly" (at 5s)
  - [ ] Silence (1.5s pause)
  - [ ] Cycle repeats

## Test 4: Voice Coaching - Affirmations
Every 15 seconds, affirmation text fades and new one is spoken:
  - [ ] First affirmation: "You are safe in this moment."
  - [ ] Wait 15s for second: "One shape at a time."
  - [ ] Voice is calm, slow (0.8x speed), slightly lower pitch

## Test 5: Sound Effects
Play the game and verify:
  - [ ] Rotation (↑ or tap): Subtle 440Hz tone (very brief)
  - [ ] Piece placement (space or drop): Gentle 349Hz tone
  - [ ] Line clear: Ascending C-E-G arpeggio (peaceful chord)

## Test 6: Background Ambience
Listen to ambient soundscape:
  - [ ] Low frequency drone (55Hz - deep, meditative)
  - [ ] Subtle harmonic at 110Hz (octave above)
  - [ ] Soft filtered noise (rain-like, peaceful)
  - [ ] Overall volume is subtle, not overpowering

## Test 7: Audio Controls
### Voice Toggle:
  - [ ] Uncheck: Voice stops immediately (ongoing speech cancelled)
  - [ ] Check: Voice resumes with next breath/affirmation

### Ambience Toggle:
  - [ ] Uncheck: Background drone/noise stops
  - [ ] Check: Ambience restarts

### Sounds Toggle:
  - [ ] Uncheck: Rotation/placement/line-clear sounds stop
  - [ ] Check: Sound effects work again

### Volume Slider:
  - [ ] Move to 0%: All audio muted
  - [ ] Move to 100%: Audio at full volume
  - [ ] Move to 50%: Mid-range volume

## Test 8: Browser Compatibility
Test in multiple browsers:
  - [ ] Chrome/Edge (best Web Audio API support)
  - [ ] Safari (check TTS voice quality)
  - [ ] Firefox (verify oscillators work)

## Test 9: Console Errors
Check browser console for:
  - [ ] No JavaScript errors
  - [ ] No Web Audio API warnings
  - [ ] No Speech Synthesis errors

## Expected Voice Characteristics
- **Rate**: 0.75-0.8 (slower than normal speech)
- **Pitch**: 0.9-0.92 (slightly lower, more soothing)
- **Voice**: Prefers female/natural/Samantha voices
- **Volume**: Matches master volume setting

## Expected Audio Levels
- Master gain: 60% (adjustable via slider)
- Ambience: 15% of master
- Effects: 25% of master
- Individual sounds:
  - Drone: 8% intensity
  - Harmonic: 4% intensity
  - Noise: 6% intensity
  - Chimes: 40% intensity (with envelope)

## Known Issues to Watch For
- TTS voices may take a moment to load on first page load
- Safari may require user interaction before playing audio
- Some browsers may have different voice options available
- Web Audio API requires user gesture to start (handled by Begin Session button)

## Pass Criteria
✓ All voice coaching speaks clearly and calmly
✓ Background ambience is subtle and peaceful
✓ Sound effects are gentle and not jarring
✓ Controls work reliably
✓ No console errors
✓ Overall experience is calming and therapeutic
