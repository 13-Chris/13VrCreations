# Riddle Panel — Quick-Use Guide (Plug-and-Play)

A drag-and-drop riddle puzzle panel for **Meta Horizon Worlds**.  
No scripting required. Works on mobile + VR (Spatial UI).

---

## Quick Start (about 1 minute)

> Tip: Make sure the world is **not running** while you wire things in the editor.

1. **Add the panel**  
   From Public Assets (or your project assets), search **“Riddle Panel”** and drag **RiddlePanel_UI** into your scene.

2. **Attach the script**  
   In **Properties → Scripts**, confirm the script **RiddlePanel** is attached (it usually is—always double-check).

3. **Enter your riddles and answers**  
   In the **Properties panel of the UI gizmo**, fill `riddle1..riddle4` and matching `solution1..solution4`.  
   - Answers must be **UPPERCASE** and **13 characters or fewer**.  
   - Set `riddleCount` to how many you filled (1–4). A riddle is chosen **randomly** each session.

4. **(Optional) Sounds and effects**  
   Link `clickSound`, `correctSound`, `wrongSound`, `switchSound`, and `solveEffect`.  
   If you have a prize prop (e.g., a **key**), link it to `prizeItem` and leave `usePrizeItem = true`.

5. **Choose your win behavior (use any or all):**  
   - **Move a Door:** set `openDoorOnSolve = true`, link `doorEntity`, choose `doorMoveType` (`rotate`, `moveUp`, `moveLeft`, `moveForward`, etc.).  
   - **Reveal a Prize (like a key):** link `prizeItem` and `solveEffect` (the item will move to the effect’s position on solve).  
   - **Unlock Other Puzzles/Areas:** set `UnlockTrigger = true`, link `unlockPuzzle` (the receiver), and keep `unlockEventName = "Unlock"` (or match your listener).

6. **(Optional) Move the Panel Itself on Solve**  
   See **“Move the Panel on Solve”** below to slide/rotate the panel away after a correct answer.

7. **Press Play and test**  
   Try a wrong answer (hear error sound), then the right answer (you’ll see **✅ SOLVED** and your chosen win behavior(s)).

> Placement tip: Place the panel at **Eye level** with clear line-of-sight for easy interaction.

---

## What the Panel Does
- Shows 1–4 customizable riddles (one is randomly chosen each session).  
- Players cycle letters and press **SUBMIT**.  
- On a correct answer, it can:  
  - **Move a door** (rotate or slide in any direction), and/or  
  - **Reveal a prize** (e.g., move a **key** to your effect location), and/or  
  - **Unlock other puzzles/areas** by sending an event to enable their trigger/gate, and/or  
  - **Move the panel itself** to a new position/rotation (e.g., slide away after solving).

---

## Essential Properties (just the basics)

| Property | Type | Default | Purpose |
|---|---|---:|---|
| `riddleCount` | Number | `4` | How many riddles you filled (1–4). |
| `riddle1..4` | String | — | Riddle text. |
| `solution1..4` | String | — | Matching answer (**UPPERCASE**, ≤ 13 chars). |
| `clickSound`, `switchSound`, `wrongSound`, `correctSound` | Entity | — | Optional UI/feedback sounds. |
| `solveEffect` | Entity | — | Optional particle effect on solve (also used as the **target position** for your prize). |
| `prizeItem` / `usePrizeItem` | Entity / Bool | `true` | Moves a prize (e.g., **key**) to the FX location on solve. |
| `openDoorOnSolve` | Bool | `false` | If true, animates a linked door on solve. |
| `doorEntity` | Entity | — | The door to animate. |
| `doorMoveType` | String | `rotate` | `rotate` \| `moveUp` \| `moveDown` \| `moveLeft` \| `moveRight` \| `moveForward` \| `moveBackward` |
| `doorRotateAxis` / `doorRotateAngle` | String / Number | `y` / `90` | Axis/angle for `rotate`. |
| `doorRaiseAmount` | Number | `3` | Distance for non-rotate moves. |
| `animDurationSeconds` | Number | `2` | Animation duration (seconds). |

**Appearance (optional):**  
`backgroundColor`, `textColor`, `panelWidth`, `panelHeight`, `buttonWidth`, `textSize`,  
`backgroundTexture(1..5)`, `backgroundSelection`, `iconTexture`.

**Need color ideas?** Try the color picker: https://htmlcolorcodes.com/color-picker/

---

## Move the Panel on Solve (optional)

You can make the **panel itself** animate to a new spot and/or rotation after a correct answer—useful to clear space or reveal hidden areas.

**Props:**
- `moveOnSolve` (Boolean): turn this on to move the panel after solving.  
- `moveTargetPosition` (Vec3): where the panel should move to (e.g., behind a wall or above the door).  
- `moveTargetRotation` (Quaternion): the rotation to end on (e.g., tilt/turn the panel as it slides away).  

**How it works:**  
When the player solves the riddle, the panel animates from its current transform to `moveTargetPosition` / `moveTargetRotation` over `animDurationSeconds`.

> If you enable `moveOnSolve` but don’t set a target position/rotation, the panel won’t have anywhere to go—be sure to set both targets for a visible result.

---

## Unlock Other Puzzles/Areas (optional)

**RiddlePanel props:**
- `UnlockTrigger` (Boolean): send an event on solve.  
- `unlockPuzzle` (Entity): the receiver (e.g., your gate/manager).  
- `unlockEventName` (String): event name (default `"Unlock"`).

**How to use it (no code on the panel):**
1. On your **receiver** object, use a script that listens for `unlockEventName` and **enables its TriggerGizmo** (unlocks the gate/area).  
2. In the panel’s Properties, set `UnlockTrigger = true`, link `unlockPuzzle` to the receiver, and keep `unlockEventName = "Unlock"` (or match your listener).

---

## Quick Door Setup (optional)

1. `openDoorOnSolve = true`  
2. Link `doorEntity`  
3. Choose `doorMoveType`  
4. Press Play → solve the riddle → door animates

---

## Testing Checklist

- Wrong answer → error sound only.  
- Correct answer → **✅ SOLVED**, success sound, and your chosen win behavior(s):  
  - Door moves, **and/or** prize (like a key) appears at your effect, **and/or** other puzzles/areas unlock, **and/or** panel moves away.  
- Still nothing? Double-check linked entities in Properties (sounds, FX, door, unlock, move targets).

---

## Troubleshooting

- **Panel not visible** → Ensure Spatial UI and preview auto-start.  
- **Letters don’t change** → Ensure no geometry blocks rays; confirm the script is attached.  
- **No audio/FX** → Linked entities must be **AudioGizmo/ParticleGizmo**.  
- **Door doesn’t move** → Check `openDoorOnSolve`, `doorEntity`, `doorMoveType`, and `animDurationSeconds`.  
- **Unlock didn’t work** → `UnlockTrigger = true`, `unlockPuzzle` is linked, receiver listens for `unlockEventName` and enables its TriggerGizmo.  
- **Panel didn’t move** → `moveOnSolve = true` and set both `moveTargetPosition` **and** `moveTargetRotation`.  
- **Solution rejected** → Must be **UPPERCASE** and **≤ 13** characters.

---

*This guide is for creators using the provided asset in their worlds (no publishing or redistribution steps included).*
