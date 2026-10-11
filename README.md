# Cue — a longevity coach in your glasses

Cue watches what your Meta glasses see (through a WhatsApp video call on your laptop) and coaches you out loud,
from morning to night.

- **All-day coach:** every few seconds an open-source vision model (Qwen2.5-VL from Hugging Face) looks at one frame
  and spots habits: water, morning sunlight, caffeine, late-night screens, scrolling, fruit and veg, junk food,
  brushing teeth, sunscreen, vitamins, stairs, reading, late meals, and bad habits to break: vaping and
  nicotine, doomscrolling, sugary drinks and betting apps. Cue logs them, nudges you by voice and keeps a
  health score for the day. A demo clock (Live / Morning / Afternoon / Night) shows how advice changes with the time.
- **Train:** bicep curls with the Cue Clip (Arduino + accelerometer + buzz) and squats from the glasses camera:
  reps, tempo, half-rep checks and set summaries. Workouts count toward the day's score.

General wellness coaching, not medical advice.

---

## 1. Install Python 3.12 (one time, 5 minutes)

1. Go to https://www.python.org/downloads/release/python-31210/
2. Scroll down, click **Windows installer (64-bit)**, run it.
3. On the first screen, **tick "Add python.exe to PATH"**, then click **Install Now**.

> Already have a newer Python (3.13+)? Still install 3.12. The hand-tracking library doesn't support 3.13 yet. Both can live side by side.

## 2. Set up Cue (one time, about 5 minutes)

1. Unzip this folder somewhere simple, like `Desktop\cue`.
2. Double-click **install.bat**.
   - If Windows says "Windows protected your PC": click **More info → Run anyway**.
3. Wait for **"Done!"**. If you see red error text, copy it and send it to Claude.

## 3. Test with your laptop camera (no glasses yet)

1. Double-click **run_webcam_test.bat**. A window called **Cue** opens.
2. **Click on the Cue window** so it gets your key presses.
3. Press **S**: you should hear "Cue voice test" from the laptop.
4. Press **K** (kitchen): hold your hand flat in front of the camera, then curl your fingertips. The label should switch between **FLAT** (red) and **curled** (green), and Cue says "Curl your fingertips" when flat.
5. Press **G** (gym): slowly tilt the laptop screen back and forth. The yellow line moves and **Reps** goes up.
6. Press **Q** to quit.

## 4. Use it with the glasses

1. Start the WhatsApp video call (iPhone → laptop) and **double-press the glasses' capture button** so the call shows your view.
2. Make the WhatsApp window big. **Move your mouse away** from the video so the call buttons hide.
3. Double-click **run_bridge.bat**.
4. The first time, a screenshot appears: **drag a box around the glasses video** (not your small self-view), press **ENTER**. It's saved; press **C** later to change it.
5. Press **Enter** for the all-day coach, or **1** curls / **2** squats.

### Getting Cue's voice into the glasses
1. Install **VB-Audio Virtual Cable** (free): https://vb-audio.com/Cable/ (run the setup as administrator, then restart).
2. In WhatsApp on the laptop: **Settings → Video & voice → Microphone → "CABLE Output"**.
3. That's it: Cue finds the cable by itself and speaks into the glasses **and** the laptop speakers.
   (To use only the speakers, set `"audio_device": "speakers"` in config.json.)

## Keys (click the Cue window first)

| Key | What it does |
|---|---|
| Enter (or 3) | Today screen: start the all-day coach |
| 1 / 2 | Bicep curls (Cue Clip) / squats (camera) |
| Z (or click Live/Morning/Afternoon/Night) | Demo clock |
| Esc (or H) | Back to the Today screen |
| Enter | After a set: next set |
| S | Voice test |
| W | Gym: read the dumbbell weight again (AMD model) |
| N | Kitchen: skip to the next step |
| C | Choose the screen area again |
| D | Show/hide technical details (fps, sensitivity) |
| R | Reset rep count |
| B | Connect the Cue Clip (USB Arduino) |
| M | Gym: count reps from the camera or the clip |
| Y | Clip rhythm on/off (buzzes the target pace) |
| T | Set the tempo target from your last 3 good reps |
| + / - | Rep sensitivity (+ = needs bigger moves, - = smaller) |
| ] / [ | Claw grip: ] = stricter, [ = more relaxed |
| Q | Quit |

## If something's off

| Problem | Fix |
|---|---|
| Keys do nothing | Click on the Cue window first |
| Reps don't count | Press **-** (minus) a few times |
| Counts reps while you stand still | Press **+** a few times |
| Says FLAT when your fingers are curled | Press **[** a few times |
| Never warns when your hand is flat | Press **]** a few times |
| Black screen / "No video" | Press **C** and drag around the glasses video |
| Any error message | Copy the whole window text and send it to Claude |
| Coach says nothing | Look at **Needs a fix** in the coach panel; it says what's wrong |
| "Cue window is covering the glasses video" | Drag the Cue window to the side so WhatsApp's video is visible |
| Glasses hear an echo of themselves | In the WhatsApp call: Speaker = laptop speakers (NOT CABLE), Mic = CABLE Output |
| "Today's free vision limit is used up" | Make a Gemini key in a NEW project at aistudio.google.com, run run_setup_vision.bat |
| Old numbers on the Today screen | Click **Start a new day** before a demo |

## The AMD vision model
1. Launch the Model API on AMD Radeon Cloud (Claude gave you the command).
2. Double-click **run_vision_test.bat**, paste the endpoint URL when asked. You want **4/4 passed**.
3. After that, Cue uses it automatically:
   - **Gym:** reads the weight on your dumbbell when you press G (or W).
   - **Kitchen:** says each step of the recipe, checks it's done, and warns if you grab the wrong spoon.
- The recipe is **skills/kitchen_basics.json**. Open it in Notepad to change the steps to match your props.

## The Cue Clip
**MLH lab parts (Arduino + Grove accelerometer + servo, on a USB cable):** follow **hardware/WIRING_USB.md**. This is the default. 3D files: **hardware/cue_case.stl** and **hardware/cue_case_lid.stl**.
**ESP32 + Bluetooth version:** follow **hardware/WIRING.md** and set `"clip_type": "ble"` in config.json.
Once the clip works with **run_clip_test.bat**, press **B** in the Cue window to connect it. Good reps get a short buzz, bad reps a long buzz, and the claw-grip warning a double buzz (the servo wiggles, plus a beep if you wired the buzzer).

Every rep and warning is saved to `session_log.csv` (used later for the dashboard).
