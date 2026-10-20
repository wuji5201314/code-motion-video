# Director-Layer Techniques for Code-Generated Motion Video

Distilled (paraphrased) from 观默 @guanmo_ai, "用 Codex 自动生成顶尖动效视频" (2026-09-30).
Source: https://x.com/guanmo_ai/status/2105146205915283737 · Gallery: https://github.com/guanmo-ai/awesome-ai-motion

## 0. The core idea

Final quality ≈ 设计判断 × 运动语言 × 素材 × 节拍与声音 × 渲染 × 自检.
If any factor is near zero, the result reads as "AI-made slides".
Counter-intuitive principle: premium feel comes from *removing* degrees of freedom.
A good brief is full of restrictions — few colors, tiny overshoot, banned template moves.

## 1. Nine concepts to know cold

- **Shot（分镜）**: cut the piece into shots; each shot has one main job and a duration.
- **Subject hierarchy（主次/背景）**: decide who is seen first, who supports, who pads the background.
- **Depth（前中后景）**: near layer grabs attention and adds depth, mid layer does the work, far layer sets mood. Flat layouts can use top/middle/bottom bands instead.
- **Stagger（错相）**: never move everything in sync. Subject first, then secondary, camera a few frames later, text/shadows later still.
- **Anticipate → impact → settle（预备→冲击→回稳）**: wind up, hit, aftershock. Never a uniform slide.
- **Overshoot（过冲/回弹）**: pass the target slightly, then pull back.
- **Light trio（主光/高光/接触影）**: where light comes from, where the bright edge sits, the contact shadow grounding objects.
- **Beat sync（卡点）**: heavy actions land on music beats.
- **BGM vs SFX**: BGM carries段落 emotion; SFX fires on contact, landing, and switch events.

## 2. The 8-slot Director Brief (fill before coding)

1. **Goal** — who watches? What do they remember afterwards?
2. **Shots & hierarchy** — what does each shot do? Who is seen first?
3. **Trajectories & camera** — where does each element travel? Why does the camera move?
4. **Motion & physics** — what exerts force? Where is contact, bounce-back?
5. **Materials & light** — what does it feel like? Where is the key light?
6. **Beat & sound** — how does the music segment? Which frame gets each SFX?
7. **Forbidden list** — which template moves/effects may never appear?
8. **Acceptance criteria** — how is "done" proven? How is a revision described?

Example values (15s product spot): 120 BPM, 60fps → ~30 frames per beat;
structure 起势 0–3s (subject enters, sets expectation) / 发展 3–11s (core demo, main
action sells the point) / 收束 11–15s (freeze on product + one key line).

## 3. Sequence map (order of work)

Beat grid → core message → shots → hierarchy & depth → physics/light/sound →
style pass → export check.

- Core: audience, core content, core expression.
- Shots: one main job per shot; even a 15s piece needs 起势→发展→收束 rhythm.
- Physics is the biggest differentiator — never skip it.
- Check: watch the whole piece; scrub frame-by-frame where it feels off.

## 4. Layering & stagger rules

- Every layer gets its own process and trajectory; backgrounds flow, never sit static.
- Stagger order: subject → secondary → camera → contact shadow → background.
- Multiple subjects behave like 发起→回应→截断→接管, not independent solos.
- **Failure signals**: all layers sharing one progress curve (strong "PPT sync" smell);
  subject, camera, shadow, text and SFX almost never share one timing function in
  real motion design; aim for one dominant event at any moment.

## 5. Motion language: the 7-phase chain

预备 (anticipate) → 加速/拉伸 (accelerate/stretch) → 冲击 (impact) →
压扁 (squash) → 过冲 (overshoot) → 回稳 (settle) → 次级响应 (secondary response).

- Cause before deformation: squash & stretch come from speed and impulse, not random easing.
- Use overshoot only where the energy justifies it.
- **Momentum continuity**: the end velocity of one segment should be the start
  velocity of the next. Resetting velocity to zero at every state change is the
  classic beginner tell — one continuous motion across states reads as far more
  sophisticated than re-popping every segment.

## 6. Physics chains (pick per action)

- **Falling & bounce**: gravity → contact → rebound.
- **Collision & inertia**: contact → force transfer → continued motion.
- **Squash & rebound**: stretch → compress → recover.
- **Bend & twist**: applied force → deformation → propagation outward.
- **Soft body & water**: disturbance → ripple → decay.
- **Particles & flow**: flow field → trajectories → gather/disperse.

Rule: motion must have an explicit force origin; contact, deformation, and
follow-through happen as one coherent chain.

## 7. Lighting checklist

- Key / weak fill / rim light; roughness; specular highlights; contact shadows; perspective.
- Landing checks: (a) key-light direction consistent across the whole piece;
  (b) key objects never "freeze" — when position stops, highlights, lit faces and
  reflections keep moving; (c) when impact reaches the environment, stagger it by
  layer instead of shaking the entire frame.

## 8. Beat, SFX & music

- Build the beat grid first. Example: 120 BPM @ 60fps ≈ 30 frames per beat.
- Land heavy actions on beats — but not every beat needs motion.
- **Micro-offsets** read as natural: anticipation slightly early, impact slightly
  off-beat, debris slightly late. Everything landing on the exact same frame reads fake.
- SFX grows out of motion: speed → whoosh, impulse → impact. Never animate first
  and paste sounds on afterwards.
- BGM handles段落 and mood; SFX marks contact/landing/switch frames.
- Judge sound by the whole phrase, not the loop — a looping pad is not sound design.
- Reference frame map: anticipation ~frame 24 → contact ~frame 30 (on the downbeat)
  → aftermath ~frame 33.

## 9. Tool notes

- **Remotion**: write video frame-by-frame in React; version-controllable project,
  mature preview/render pipeline. Its docs treat coding agents as first-class users.
- **HyperFrames**: auto-assembles scripts/assets into finished pieces; good for
  explainer voiceovers, step cards, fast turnaround.
- **ChatCut**: conversational editing — change timelines and captions in natural language.

## 10. Prompting the agent

Give the coding agent the whole director stack as tasks, not just "make an animation":
reference & shots, motion & physics, light & sound, review & revision. Without the
director layer in the prompt, the agent only "builds it"; with it, the agent can
also direct pacing, weight, and restraint. More code is a rough (not absolute)
proxy for detail: richer pieces tend to have more code because every layer owns
its own timing functions.
