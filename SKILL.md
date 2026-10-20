

# Code Motion Video

## Purpose
Turn a code-capable agent into a motion-video director. The quality gap in AI motion video is not the model — it is whether the agent was fed the full "director layer" (reference, shots, beats, physics, layering, sound, self-checks). Without it, output regresses to defaults: centered big text, neon gradients, everything fading in sync.

Method distilled from 观默 @guanmo_ai, "用 Codex 自动生成顶尖动效视频" (2026-09-30): https://x.com/guanmo_ai/status/2105146205915283737
Reference gallery of AI motion work: https://github.com/guanmo-ai/awesome-ai-motion

## Workflow
1. **Write the Director Brief before any code.** Fill all 8 slots: goal, shots & hierarchy, trajectories & camera, motion & physics, materials & light, beat & sound, forbidden list, acceptance criteria. If the user cannot fill them, draft them and ask for review. Template and example values: `references/techniques.md`.
2. **Build the sequence map.** Work in this order: beat grid → core message → shots → hierarchy & depth → physics/light/sound → style pass → export check. For a ~15s spot use 起势 0–3s / 发展 3–11s / 收束 11–15s.
3. **Stagger layers, never sync them.** Main subject moves first, secondary layer lags, camera a few frames later, shadows later still. One dominant event per moment; no two layers share one progress curve.
4. **Write motion as cause → effect.** Full chain per action: anticipate → accelerate/stretch → impact → squash → overshoot → settle → secondary response. Keep momentum continuous: a segment's end velocity becomes the next segment's start velocity.
5. **Give every motion a physical cause.** Gravity arcs, collisions, springs, deformation — pick from the physics chains in `references/techniques.md`. No unexplained easing.
6. **Light deliberately.** One key-light direction held across the whole piece. Key objects keep moving highlights even when static. Stagger environmental shake by layer; never shake the whole frame at once.
7. **Sync to a beat grid, then micro-offset.** Build the frame grid from BPM first (e.g. 120 BPM @ 60fps ≈ 30 frames per beat), land heavy actions on beats, then offset: anticipation slightly early, impact slightly off, debris slightly late. SFX grows out of motion — speed→whoosh, impulse→impact — placed on contact/landing/switch frames.
8. **Self-review like a director.** Render, watch a full pass, then frame-step the weak spots. Reject on failure signals: all layers sharing one timing curve, frozen highlights on static objects, whole-frame shake, template glow/gradient looks.

## Output Contract
- A Director Brief (8 slots filled) exists before code is written.
- Generated code encodes per-layer timing functions, physical causes, and beat-aligned events — not one global easing.
- A review pass ends with explicit accept/rework notes against the brief's acceptance criteria.

## Operating Rules
1. No code before the 8-slot brief is filled.
2. Constraints beat freedom: the brief must carry a forbidden list banning default looks (centered big text, neon gradients, all-elements-fade-in-sync, identical motion for all layers).
3. One main event at a time; stagger everything else.
4. Every squash/stretch/overshoot needs a physical cause named in the brief.
5. SFX is placed on event frames during planning, never pasted on after animation is done.
6. Pairs with `remotion-best-practices` (implementation) and `ffmpeg-skill` (sound mix, loudness, post) — this skill owns direction, they own execution.
7. When the result looks like "AI-made slides", the failure is a missing director layer: re-brief, don't just re-render.
## Available connectors

Use `muse.connector_search` to find the current connector docs before following
recovered instructions.
