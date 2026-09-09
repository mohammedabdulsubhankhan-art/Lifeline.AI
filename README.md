# Lifeline.AI 
AI Vitals Guardian

Part of the **LifeLine AI** — AI in Allied Healthcare exhibit.

An AI monitoring prototype for elderly / at-risk patients living alone. Wearable
vitals (heart rate, SpO₂, blood pressure, sleep) are watched continuously. If a
reading goes abnormal, the AI "checks in" with the patient directly. If the
patient responds, nothing happens. If there's no response before the timer
runs out, family and emergency contacts are notified automatically.

## Files

| File | What it is |
|---|---|
| `Lifeline.AI` | The whole app — single React component, no external files needed |

## How it's built

- **React** (function components + hooks — `useState`, `useEffect`, `useCallback`)
- **Tailwind** utility classes for layout/spacing
- **Inline SVG** for the sparkline charts, countdown ring, and heartbeat line
  (no charting library — kept dependency-free on purpose)
- **Google Fonts** loaded at runtime: Fraunces (headers), Inter (body), IBM Plex Mono (numbers/timestamps)

No backend, no real device connection — vitals are simulated in-browser so it
can be demoed anywhere, including offline.

## How the demo flow works

1. Vitals drift slightly on their own every ~2 seconds (`useEffect` interval) — this is just to make numbers feel "live."
2. Clicking a **demo control** button (bottom of page) forces one vital abnormal and sets `status` to `"checking"`.
3. That opens the **AI check-in panel** with a 15-second countdown (`checkin.remaining`).
4. Two outcomes:
   - **"Simulate: patient responds"** button → `patientResponds()` → status resolves, vitals reset to baseline.
   - **Timer hits 0** → `escalate()` fires automatically → status becomes `"alert"`, contacts get marked `notified: true` one by one.
5. **Reset demo** button puts everything back to the starting state for the next run-through.

## Where to look if you want to change things

- **Colors** — all in the `COLORS` object at the top. Change one hex, it updates everywhere.
- **Patient name/age** — hardcoded in the header JSX (`Mrs. Fatima, 72`) and inside `CONTACTS` / the check-in message text.
- **Emergency contacts** — edit the `CONTACTS` array near the bottom (name + role only, `notified` is added automatically).
- **Countdown length** — change `total: 15` inside `triggerSpo2Drop()` / `triggerHrSpike()`.
- **AI check-in message text** — inside the check-in panel JSX, the `checkin.trigger === "spo2" ? "..." : "..."` line.
- **Normal vital ranges / thresholds** — the `note` props in each `VitalCard` (e.g. `hr > 120`, `spo2 < 92`).

## Known limitations (say this upfront if judges ask)

- Vitals are simulated, not pulled from a real wearable — this is a UX/logic prototype, not a hardware integration.
- No real SMS/call is sent to contacts; "Notified" is a visual state change only.
- No persistence — refreshing the page resets everything (by design, for repeatable demos).

## Suggested 30-second pitch order

1. Show the dashboard at rest — "this is what a family member or health worker would see."
2. Click **Simulate SpO₂ drop** — point out the AI check-in message and countdown.
3. Let it time out once → show the alert + contacts getting notified.
4. Click **Reset demo**, run it again but hit **"patient responds"** this time → show that it quietly resolves with *no* false alarm.
