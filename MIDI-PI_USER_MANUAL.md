# MIDI-PI Portable MIDI Player - User Manual

## Table of Contents
1. [Introduction](#introduction)
2. [Controls](#controls)
3. [Main Playback Screen](#main-playback-screen)
4. [File Browser](#file-browser)
5. [Channel Settings Menu](#channel-settings-menu)
6. [MIDI Settings Menu](#midi-settings-menu)
7. [Clock Settings Menu](#clock-settings-menu)
8. [Visualizer Mode](#visualizer-mode)
9. [Settings Files](#settings-files)
10. [Tips & Tricks](#tips--tricks)

---

## Introduction

The MIDI-PI is a portable hardware MIDI file player based on the Raspberry Pi Pico. It plays standard MIDI files (.mid, .midi) from an SD card and outputs MIDI data to external synthesizers, sound modules, or MIDI interfaces.

### Key Features
- Standard MIDI file playback (Type 0 and Type 1)
- Per-file settings (save custom configurations for each song)
- Real-time tempo control (50-200%)
- Global velocity scaling (1-100)
- 16-channel mixer with per-channel control
- Multiple playback modes (Single, Auto-Next, Loop One, Loop All)
- MIDI Thru and Keyboard modes
- MIDI Clock output
- Real-time 16-channel activity visualizer

---

## Controls

### Button Layout
- **UP/DOWN** - Navigate menus, select files, scroll options
- **LEFT/RIGHT** - Adjust values, navigate options horizontally
- **OK** - Confirm selection, activate/deactivate editing mode
- **PLAY** - Start/resume playback
- **STOP** - Stop playback
- **PAUSE** - Pause playback (press PLAY to resume)
- **MODE** - Cycle through different screens and menus
- **PANIC** - Send "All Notes Off" to all channels (emergency stop)

---

## Main Playback Screen

The main screen displays during playback and provides quick access to essential controls.

### Display Elements

```
Track: SONG_NAME.MID
00:45 / 03:30  ► SNG
BPM:120  V:50  T:100%
```

**Line 1: Track Information**
- **Track:** Currently playing MIDI file name

**Line 2: Progress & Mode**
- **00:45** - Current playback position (MM:SS)
- **/ 03:30** - Total song length
- **►** - Playback state icon
  - **►** Playing
  - **❚❚** Paused
  - **■** Stopped
- **SNG** - Playback mode (see below)

**Line 3: Playback Parameters**
- **BPM:120** - Current tempo in beats per minute
- **V:50** - Global velocity scale (50 = normal)
- **T:100%** - Tempo percentage (100% = normal speed)

### Playback Modes

Navigate using LEFT/RIGHT when "MODE" option is selected:

- **SNG** (Single) - Play current track once, then stop
- **NXT** (Auto-Next) - Play current track, then automatically play next track
- **LP1** (Loop One) - Repeat current track indefinitely
- **LPA** (Loop All) - Play all tracks in sequence, then loop back to first

### Menu Options

Navigate with LEFT/RIGHT, activate with OK:

1. **TRACK** - Press OK to open file browser
2. **BPM** - Adjust tempo by ±1 BPM (adjusts tempo percentage to match)
3. **VELOCITY** - Adjust global velocity scale (1-100)
   - Values below 50: Quieter/softer notes
   - 50: Normal MIDI velocity (default)
   - Values above 50: Louder/harder notes
4. **TIME** - Fast forward/rewind 1 second per button press
5. **MODE** - Change playback mode (SNG/NXT/LP1/LPA)

### Quick Actions
- **PLAY** while stopped - Resume last played file
- **STOP + PLAY** - Reload current file settings and restart
- **MODE button** - Cycle to Channel Settings menu
- **PANIC button** - Send "All Notes Off" on all 16 channels

---

## File Browser

Access by pressing OK on "TRACK" option from main playback screen.

### Display

```
♪ MIDI Files
> SONG01.MID
  SONG02.MID
  SONG03.MID
```

### Controls
- **UP/DOWN** - Navigate file list
- **OK** - Load selected file and return to playback screen
- **PLAY** - Load selected file and start playing immediately
- **MODE** - Cancel and return to playback screen

### File Organization
- Place MIDI files in the `/MIDI` folder on your SD card
- Supported formats: `.mid`, `.midi`
- Files are sorted alphabetically
- The folder will be created automatically if it doesn't exist

---

## Channel Settings Menu

Advanced per-track and per-channel MIDI configuration. Access by pressing MODE from main playback screen.

### Display Layout

```
[SAVE]    [DELETE]
Ch: 1  M:OF  Ve:50
P:--  Pa: 64  Vo:100
```

### Top Row: Actions
- **[SAVE]** - Save current settings to a .cfg file
- **[DELETE]** - Delete saved settings file for this track

### Line 1: Channel & Global Settings
- **Ch: 1** - Selected channel (1-16)
- **M:OF** - Mute status
  - **OF** = Channel unmuted (normal)
  - **ON** = Channel muted (silent)
- **Ve:50** - **Global Velocity** (1-100, affects entire song)
  - Adjusts note dynamics for the whole track
  - Lower = softer/quieter, Higher = louder/harder
  - Default: 50 (normal)
  - Use this to balance loud/quiet tracks

### Line 2: Per-Channel Settings
- **P:--** - Program (Instrument)
  - **--** = Use MIDI file default
  - **0-127** = Override with specific instrument
  - Cycles 0→127→-- (use MIDI file)
- **Pa:64** - Pan position (stereo positioning)
  - **--** = Use MIDI file default
  - **0** = Full left
  - **64** = Center (default)
  - **127** = Full right
- **Vo:100** - Per-channel Volume (0-127)
  - **--** = Use MIDI file default
  - **0** = Silent
  - **127** = Maximum volume
  - Default: 127

### Navigation
- **LEFT/RIGHT** - Navigate between options
- **OK** - Activate/deactivate editing mode (box fills when active)
- **UP/DOWN** - When Program option active, scroll through channels
- **LEFT/RIGHT** - When option active, adjust value

### How Settings Work

**Velocity (Ve) vs Volume (Vo):**
- **Velocity (Ve)** - Global setting affecting how "hard" all notes are played
  - Affects note attack and perceived loudness
  - Changes the dynamics of the entire song
  - Use this to balance overall track volume
- **Volume (Vo)** - Per-channel MIDI CC7 volume control
  - Adjusts the volume of individual channels
  - Use this to balance instruments within a song

**Override Behavior:**
When you set a value other than the default (--):
- Your setting **overrides** the MIDI file
- The MIDI file's Program Changes, Volume (CC7), and Pan (CC10) messages are ignored
- This allows you to permanently fix problematic tracks

### Saving Settings

1. Adjust your desired settings
2. Navigate to **[SAVE]** option
3. Press **OK**
4. Settings are saved to `SONGNAME.cfg` on the SD card
5. Settings automatically load when this track plays

**Settings are saved per-file and include:**
- Channel mutes (all 16 channels)
- Program overrides (all 16 channels)
- Pan overrides (all 16 channels)
- Volume overrides (all 16 channels)
- Global velocity scale

### Deleting Settings

1. Navigate to **[DELETE]** option
2. Press **OK**
3. Confirmation message appears
4. Track reverts to default settings immediately

**Messages:**
- "Settings Deleted!" - File was deleted successfully
- "No Settings to Delete" - No .cfg file exists for this track

### Quick Actions
- **MODE button** - Cycle to MIDI Settings menu
- **PANIC button** - Send "All Notes Off" on all channels
- **STOP + PLAY** - Reload settings from file

---

## MIDI Settings Menu

Configure MIDI input behavior. Access by pressing MODE from Channel Settings.

### Display

```
MIDI IN
Thru:      [OFF]
Keyboard:  [OFF]
KB Ch:      1
```

### Options

**MIDI Thru** (Thru)
- **OFF** - MIDI input ignored (default)
- **ON** - All received MIDI messages are echoed to MIDI output
- Use case: Pass MIDI from a controller through to your synth
- Note: Thru and Keyboard modes are mutually exclusive

**MIDI Keyboard** (Keyboard)
- **OFF** - Keyboard mode disabled (default)
- **ON** - Received MIDI notes play on selected keyboard channel
- Use case: Play your synth manually while file playback is stopped
- Keyboard channel: 1-16 (selectable)
- Note: Thru and Keyboard modes are mutually exclusive

**Keyboard Channel** (KB Ch)
- Selects which MIDI channel (1-16) keyboard notes are sent on
- Only active when Keyboard mode is ON

### Controls
- **LEFT/RIGHT** - Navigate options
- **OK** - Activate editing mode
- **LEFT/RIGHT (when active)** - Change value
- **MODE button** - Cycle to Clock Settings menu
- **PANIC button** - Send "All Notes Off" on all channels

---

## Clock Settings Menu

Configure MIDI Clock output. Access by pressing MODE from MIDI Settings.

### Display

```
MIDI CLOCK
Enabled:   [OFF]
```

### Option

**MIDI Clock Enabled**
- **OFF** - No MIDI clock messages sent (default)
- **ON** - Send MIDI Clock, Start, Stop, and Continue messages
- Use case: Sync external sequencers, drum machines, or DAWs to playback

### MIDI Clock Behavior

When enabled, the player sends:
- **Clock messages** - 24 pulses per quarter note during playback
- **Start message** - When playback begins from the start
- **Stop message** - When playback stops
- **Continue message** - When resuming from pause

**Note:** MIDI Clock is only sent during active playback.

### Controls
- **LEFT/RIGHT** - Navigate option (only one option)
- **OK** - Activate editing mode
- **LEFT/RIGHT (when active)** - Toggle ON/OFF
- **MODE button** - Cycle to Visualizer
- **PANIC button** - Send "All Notes Off" on all channels

---

## Visualizer Mode

Real-time 16-channel MIDI activity display. Access by pressing MODE from Clock Settings.

### Display

```
 1  2  3  4  5  6  7  8
█  ▆  █  ▄     ▂  █  ▅
 9 10 11 12 13 14 15 16
▃     █  ▆  ▂     ▄  █
```

### What It Shows
- **16 columns** - One for each MIDI channel (1-16)
- **Bar height** - Current note activity (velocity average)
- **Peak indicator** - Highest recent activity (holds for 800ms)

### Activity Detection
- Bars respond to Note On messages
- Height represents average velocity of active notes
- Polyphonic notes are averaged together
- Bars decay smoothly when notes end
- Sensitivity scaled so max velocity reaches ~80% height

### Channel Notes
- **Channel 10** - Usually drums/percussion in General MIDI
- **All channels** - Activity shown regardless of mute status

### Controls
- **MODE button** - Cycle back to main playback screen
- **STOP** - Clear visualizer bars
- **PAUSE** - Clear visualizer bars
- **PANIC button** - Send "All Notes Off" on all channels

### Use Cases
- Verify MIDI file is playing correctly
- Identify which channels are active
- Troubleshoot silent channels (check if bars appear)
- Visual feedback during setup

---

## Settings Files

The MIDI-PI creates `.cfg` files to store per-track settings.

### File Format
- Filename: `SONGNAME.cfg` (matches MIDI file name)
- Location: Same folder as MIDI file
- Format: Plain text key=value pairs
- Encoding: ASCII

### Example Content
```
[MIDI_SETTINGS_V1]
MUTES=0
PROGRAMS=128,128,128,0,128,128,128,128,128,128,128,128,128,128,128,128
VOLUMES=255,255,255,100,255,255,255,255,255,255,255,255,255,255,255,255
PAN=255,255,255,64,255,255,255,255,255,255,255,255,255,255,255,255
VELOCITY_SCALE=50
```

### Value Meanings
- **MUTES** - 16-bit bitmask (bit set = channel muted)
- **PROGRAMS** - Comma-separated values per channel
  - 0-127: Specific program number
  - 128: Use MIDI file default
- **VOLUMES** - Comma-separated values per channel
  - 0-127: Specific volume level
  - 255: Use MIDI file default
- **PAN** - Comma-separated values per channel
  - 0-127: Specific pan position (0=left, 64=center, 127=right)
  - 255: Use MIDI file default
- **VELOCITY_SCALE** - Global velocity (1-100)

### Manual Editing
You can edit `.cfg` files directly on your computer:
1. Remove SD card and insert into computer
2. Edit `.cfg` file with text editor
3. Save changes
4. Re-insert SD card into MIDI-PI
5. Load the track - settings apply automatically

---

## Tips & Tricks

### Performance Tips

**Balancing Loud/Quiet Tracks**
- Use global Velocity (Ve) setting in Channel Settings
- Save settings per-file for consistent playback
- Start with Ve:30-40 for very loud tracks
- Start with Ve:60-70 for quiet tracks

**Fixing Problematic Instruments**
- Override Program on specific channels
- Experiment with different programs (0-127)
- Save settings to avoid re-configuration

**Creating Custom Mixes**
- Mute backing channels to create karaoke tracks
- Adjust per-channel Volume (Vo) to balance instruments
- Use Pan to create wider stereo image

### Workflow Tips

**Fast Navigation**
- Press PLAY from file browser to load and play immediately
- Use Auto-Next (NXT) mode for continuous playback
- Use Loop All (LPA) mode for background music

**Tempo Adjustments**
- Tempo changes are temporary (not saved)
- Reload file (STOP + PLAY) to reset tempo to 100%
- Use BPM control for fine adjustments (±1 BPM)
- Use TIME for quick navigation during playback

**Testing Changes**
- Use visualizer to verify channel activity
- PANIC button immediately silences stuck notes
- STOP + PLAY reloads settings from .cfg file

### Troubleshooting

**No Sound**
1. Check MIDI cable connections
2. Verify channel isn't muted (M:OF = unmuted)
3. Check visualizer - do bars appear?
4. Verify channel volume (Vo) isn't 0
5. Try deleting .cfg file and reloading

**Stuck Notes**
- Press PANIC button (sends All Notes Off)
- Check MIDI cable quality
- Verify synth is receiving on correct channels

**File Won't Load**
- Ensure file is in `/MIDI` folder
- Check file extension is `.mid` or `.midi`
- Try file with shorter filename
- Verify SD card is formatted FAT32

**Settings Not Saving**
- Ensure SD card isn't write-protected
- Check SD card has free space
- Try a different SD card
- Verify filename doesn't have special characters

**Seeking Issues (Long Files)**
- Fast forward/rewind may be slower on very long files
- Device automatically pauses during seeking to prevent MIDI leakage
- Limit is 50,000 events per seek operation (safety feature)

### SD Card Recommendations
- Format: FAT32
- Speed: Class 10 or better recommended
- Size: 2GB-32GB (larger works but unnecessary)
- Quality: Use reputable brand cards to avoid corruption

### MIDI File Recommendations
- Format: Type 0 or Type 1 MIDI files
- Resolution: 96-480 PPQN recommended
- Size: Files under 1MB work best
- Tempo: Files with embedded tempo changes supported

---

## Appendix: MIDI Technical Details

### MIDI Implementation

**Output:**
- Standard MIDI 1.0 specification
- Baud rate: 31,250 bps
- Connector: 5-pin DIN or TRS (depending on hardware)
- All 16 channels supported
- All standard MIDI messages supported

**Supported MIDI Messages:**
- Note On/Off
- Program Change
- Control Change (all CCs)
- Pitch Bend
- Channel Aftertouch
- Polyphonic Aftertouch
- System Exclusive (SysEx)
- MIDI Clock (when enabled)
- MIDI Start/Stop/Continue (when clock enabled)

**Filtered Messages (when overrides set):**
- Program Change (when user program 0-127 set)
- CC7 Volume (when user volume 0-127 set)
- CC10 Pan (when user pan 0-127 set)

**MT-32 Compatibility:**
- SysEx messages sent with 35ms delay
- Suitable for MT-32, MT-32 Pi, or similar modules
- Works with FluidSynth mode on MT-32 Pi

### Hardware Specifications

**Processor:** Raspberry Pi Pico (RP2040)
- Dual-core ARM Cortex-M0+ @ 133MHz
- Core 0: UI, display, file I/O
- Core 1: MIDI timing (dedicated for accuracy)

**Storage:** MicroSD card (FAT32)
- SD card speed: 12 MHz SPI clock
- Supports cards up to 32GB (SDHC)

**Display:** 128x64 OLED (SSD1306)
- I2C interface
- Monochrome graphics

**Timing Accuracy:**
- Microsecond-precision MIDI timing
- Tempo range: 50-200% (BPM dependent)
- MIDI Clock: 24 PPQN

---

## Version Information

**Firmware Version:** 1.0
**Manual Version:** 1.0
**Last Updated:** 2025

For support, bug reports, or feature requests, please contact the manufacturer or visit the project repository.

---

**End of Manual**
