## 0.11.1 (unreleased)

## 0.11.0

### Features

- Implement split stereo bar colors (#491)
- Add "Reload Font List" menu item to fix missing fonts (#492)
- Remember most recently selected filetype in Render dialog (#493, #500)

### Major Changes

- Encode audio using libopus instead of aac (#487)
- Fix loss of volume when using a mono track as master audio (#488, #510)
- Fix bug where previewing project files breaks them (#509)

### Changelog

- When opening missing file via CLI, show dialog rather than crashing (#499)
- Fix saving global settings after opening config in Unicode folder (#507)
- Save config files atomically (#507)
- Switch from Poetry to uv project manager (#508)

## 0.10.1

### Changelog

- Raise minimum Python version to 3.10, fix NumPy 2.0 support (#480)

## 0.10.0

### Features

- Add support for showing stereo balance as bars (#475)
- Add external triggering (#476)

### Major Changes

- Switch default track orientation to horizontal (#477)

### Changelog

- Add warning when rendering partial video (#478)

## 0.9.1

### Major Changes

- Fix bug which would add correlation buffers to slope finder (#471, found by Tachometer)
    - NOTE: This affects triggering behavior compared to older program versions.
- Fix stray triggering results in long areas of audio with no rising edges (#474)

## 0.9.0

### Features

- Add multi-core rendering for increased preview/render speed (#450)

### Major Changes

- Update defaults (#458)
    - Switch default stereo orientation to vertical (to match user expectations)
    - Switch to 1080p output resolution by default, increase label font size
    - Enable track labels by default (from filename)
- Use automatic column count by default, remove empty rows/columns (#461)
    - If both row and column count are set to 0 (Auto), corrscope picks 2 columns when rendering 5 or more tracks. If you set the row or column count to a specific number, it will be used as before.
    - Change default UI text for row/column count from blank to Auto, to improve usability.

### Changelog

- Improve error messages for 24-bit WAV files (#443)
- Add end time field to GUI (#451)
- Change GUI render divisor to 4 decimal places (#451)
- Fix crash with pitch tracking on low-sample-rate channels (#453)
- Fix crash on macOS when closing window with preview active (#454)
- Fix bug where background images had 1px black borders on top/left and were shown 1px too big (#464)

## 0.8.1

### Changelog

- Fix slowdown on (M1?) Macs around 40 seconds after preview/render starts (#428)

## 0.8.0

This major release introduces new visual options (pitch coloring and background images), Mac and M1 support, and a triggering rewrite based on new and improved algorithms. This release is partly incompatible with previous config files (slope triggering strength was removed, triggering will not behave in the same way). Be sure to check out the updated help and tutorial ([link](https://corrscope.github.io/corrscope/))!

Note that when corrscope is running on M1 Mac processors, corrscope's preview will slow down if you focus the preview window rather than the corrscope window. There is no fix for this issue at the moment. As a workaround, you can click on Corrscope's window to avoid the slowdown, and drag it aside so it doesn't obstruct the preview.

### Features

- Add option to color lines by pitch (#386)
- Add support for background images (#388, @Sanqui)
- Add support for line outlines (#388, @Sanqui)
- Add Mac and M1 support (#415, @beetrootpaul)

### Major Changes

- Rewrite the trigger algorithm to enhance determinism and reduce errors when DC offset varies within a frame (#403, #408, #416, #420)
    - Slope strength has been removed and folded into edge strength (#416). This should *usually* not reduce the ability to fine-tune triggering; if it does, let me know so I can reconsider this decision!
    - Add control for DC removal rate (#408)
    - Add control to reset buffer on new notes, when wave lines up poorly with buffer (#416)
    - For more information, see help page ([link](https://corrscope.github.io/corrscope/)).

### Changelog

- Fix passing absolute .wav paths into corrscope CLI (#398)
- Fix preview error when clearing "Trigger/Render Width" table cells (#407)
- Reorganize GUI with edge triggering options before buffer (#416)
- Reorganize YAML field order based on GUI (#421)

## 0.7.1

### Major Changes

- Change `-r/--render` command line flag to take an output path (#382)
- Render videos in BT.709 colorspace by default (#384)

### Changelog

- Update NumPy so `poetry install` on Python 3.8+ won't build NumPy from source (#371)
- Fix longstanding crash when prefs.yaml is corrupted, reset settings instead (#377)
- Atomically save prefs.yaml to prevent file corruption (#377)
- Fix issue where foobar2000 WAV files fail with message "ValueError: Incomplete wav chunk." (#379)
- Build Win32 binaries as well as Win64 (#381)
- Build official Win32/Win64 binaries on Python 3.8 (the last release to support Windows 7) (#381)
- Add .mkv/.webm extensions to "Render to Video" dialog (#383)

## 0.7.0

Long delayed as well. I haven't been around corrscope in a while. Background images and spectral coloring are not in this release, but I wanted to push this out because it enables chroma subsampling (reduces support queries) and fixes the FFmpeg URL.

### Major Changes

- Enable chroma subsampling by default (may affect saved projects) (#331)
- Improve FPS by reducing rendering overhead (#335)

### Changelog

- Add menu items linking to config folder and Github repository (#343)
- Fix FFmpeg URL, switch to static FFmpeg to reduce user error (#332, #358)
- Fix bug where videos were truncated if first channel was shorter than the rest (#360)

## 0.6.1

Long delayed... sorry.

### Major Changes

- Fix bug where narrow pulse waves were erroneously detected as silence (#306)
- Fix Windows-only crash when opening a non-ASCII path and restarting corrscope (#311)
- Fix bug where unrecognized fonts would cause corrscope to crash (#313)
- Fix bug where `pip install corrscope` failed on Linux because `PyQt5-sip` was pinned to 4.x (#319)

### Changelog

- On Windows, use locale-specific font, not hard-coded Segoe UI (#322)

## 0.6.0

### Features

- Rewrite pitch tracking to avoid false negatives (#274)
    - Previously, we rescaled the *buffer* to maximize spectral similarity between *data 2 frames ago* and data now.
    - Now we rescale the buffer to maximize spectral similarity between the buffer and data now.
- Improve period calculation, add maximum frequency cap (#294)
    - Fixes incorrectly high frequency with low bass notes
    - Fixes incorrectly high frequency with treble-heavy waveforms

### Major Changes

- Update default options for new projects (#275)
    - Stereo grid opacity = 0.25
    - Render FPS Divisor = 2 (preview-only, faster)
    - Trigger Width = 60 ms
- Always enable midline color, remove color checkbox (#291)
    - Can be disabled separately for h/v
- Enable grid color #55aaff for new projects (#300)

### Changelog

- Increase GUI maximum Trigger/Render Width to 200 ms
- Update trigger GUI, merge all edge-related triggers (#299)
- Rewrite FPS printing code
- Add test to ensure cancelling render terminates FFmpeg quickly
- Add support for excluding fields from always_dump="*" (#268)
    - Don't dump viewport_width/height by default

## 0.5.1

This is a bugfix release, since master has regressions in pitch tracking.

### Changelog

- Improve GUI dialog path defaults (#277)
- Display all GUI errors in dialog box, instead of crashing (#279)
- Display dialog and terminate ffmpeg, when closing project with preview/render active (#280)


## 0.5.0

### Breaking Changes

- Reorganize GUI, move trigger options to tab
- Improve NES triangle triggering, switch data window to Gaussian (#244)
- Remove mean responsiveness (always set to 1)
    - To improve triangle waves, use sign triggering instead.

### Features

- Add sign triggering (37d2c08a)
- Add support for per-channel labels (#256)
    - Some fonts may not work or display the wrong weight, due to Matplotlib issues.
- Add configurable grid line width (#265)
- Add Ctrl+Tab or Ctrl+PageUp/Down shortcuts to switch GUI tabs (#246)

### Changelog

- Quit GUI when pressing Ctrl-C in terminal (#252)
- Rewrite resolution division system to use internal DPI (#264)
- Refactor renderer API (c8239558)
- Add renderer debugging visualizations (development only)


## 0.4.0

### Breaking Changes

- Set default mean responsiveness to 0.05 instead of 1 (even in unmodified older files, oops)
- Always use full-resolution rendering, when rendering to file (make trigger/render subsampling preview-only)
- Remove buffer falloff from GUI (defaults to 0.5)
- Lag prevention is no longer increased, when trigger subsampling or trigger width × are >1

### Features

- Add Help menu (online help manual)
- Add custom stereo downmix modes
    - Allow left-only triggering, downmixing specific channels to mono, etc.
- Add post-triggering for finding zero-crossing edges
- Add optional slope-based triggering
    - Previous edge-triggering was area-based and located zero crossings

### Changelog

- Add trigger "buffer responsiveness" option
- Remove dependency on more_itertools


## 0.3.1

### Breaking Changes
- Fix time-traveling bug by reverting "Increase trigger diameter to improve bass stability" from 0.3.0

### Changelog

- Rebuild UI in Python, not .ui XML
- Show stack trace dialog when loading config fails
- Show stack trace dialog if exceptions raised before playback begins


## 0.3.0

### Breaking Changes

- Increase trigger diameter to improve bass stability
- Line width is now measured in pixels. (previously, 1 pt = 4/3 px)
- Tweak default config settings: white lines, gray midlines, 1 column, vertical orientation.
    - Enable preview-only resolution divisor by default, to boost speed

### Features

- Add pitch-tracking trigger checkbox
    - Waves should no longer jump around when pitch changes.
    - Rapidly repeating pitch changes (less than 6 frames apart) are skipped for performance.
    - Pitch tracking may increase CPU usage on noise channels. See https://github.com/corrscope/corrscope/issues/213 for details
- Add stereo rendering support
    - Located in stereo tab in GUI
- Add per-channel amplification support
- Add color picker to GUI
- Add unit suffixes to GUI spinboxes

### Changelog

- Prevent YAML dump from line-breaking long paths
- Dump Config.show_internals to YAML by default
- Add non-GUI option to disable antialiasing (does not improve performance)


## 0.2.0 and before

See https://github.com/corrscope/corrscope/releases.
