
Yes — slate is exactly the designed fallback, and it's broken

The spec is unambiguous. specs/media-pipeline/source.spec.md:
- :96 — video lane is DENSE, filled with "slate TS slices (I-only asset)"
- :325 — "every trigger is non-fatal — dead-FCS or deadline boots into slate"
- :701 — "content late / absent / poison → slate / silence / GAP"

And the assembler did decide to air slate, correctly: the boot gate expired on deadline (source.gate_released trigger=deadline waited_ms=14858), then source.air_kind lane=video kind=slate, lane=audio_0 kind=slate, lane_aired on all three, source.air_reached took_ms=15008. The decision logic is fine. The slate just never reaches the wire.

Why — the chain, all from the pod's own records

┌────────────────────────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│          Step          │                                                                                      Evidence                                                                                       │
├────────────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ det_v0 locks the right │ detessify_pid_locked lane=det_v0 pid=256                                                                                                                                            │
│  PID                   │                                                                                                                                                                                     │
├────────────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ …then floods CC gaps   │ 5624 × detessify_cc_gap lane=det_v0, all inside one second — 0:00:15, the exact second slate first ingested                                                                         │
├────────────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ each gap discards the  │ demux.cpp:303-305 — out.push_back(kCcGap); pes_buf_.clear(); pes_active_ = false;                                                                                                   │
│ PES                    │                                                                                                                                                                                     │
├────────────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ so video emits nothing │ restamp_run_started fires for restamp_a0 — never for restamp_v0. Nothing got past det_v0 → capsfilter → h264parse                                                                   │
├────────────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ no video ⇒ no PCR      │ muxer is gsttsmuxer … pcr-pid=256 — PCR comes from the video PID                                                                                                                    │
├────────────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ sink waits forever     │ amgudpsink2 … use-first-pcr-at=true timing-mode=2 latency=3000000000. It logged init (2 outputs, 127.0.0.1:19020/19021, iface lo) and then nothing — zero late-sends, zero          │
│                        │ buffering-low, i.e. no buffer ever arrived                                                                                                                                          │
├────────────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ tardis                 │ bitrate 0.000, "No input"                                                                                                                                                           │
└────────────────────────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘

Audio was healthy end-to-end into the muxer — det_a0 locked PID 257, zero cc_gaps, restamp_a0 started. It's the video lane alone that fails, and because video carries PCR, it takes the whole mux down with it.

Correction to what I told you last turn: I ranked the cc_gap flood as a logging-hygiene issue, "harmless on a healthy channel." That was wrong. The gaps aren't noise — each one throws away a video PES, and they are the direct reason the channel emits zero bytes. The log volume is the secondary symptom; the dropped PESes are the defect.

What I have not established

Why PID 256 trips CC gaps at all. Three inputs I verified as clean, so none of them is the culprit:

- The asset is conformant. PMT: PID 256 = 0x1B H.264, PID 257 = 0x06 + registration descriptor = SMPTE-302M. PID 256's CC chain is spec-correct in file order — increments only on payload packets, AF-only packets repeat it, 13 PUSI frames, no internal discontinuity. And no SLATE_INVALID bus error fired, so it passed the assembler's own boot conformance check (source.spec.md:696).
- The demux parse is correct. demux.cpp:163-184 derives has_payload/has_adaptation/discontinuity per ISO 13818-1, and process_packet:337 excludes null and AF-only packets before the CC check.

The remaining suspect is the bit in between: per source.spec.md:96 the source doesn't push the file, it pushes slices built from a slate index (spec tests 009 "slate index: frame offsets/count correct for known asset" and 012 "single-frame slate asset loops indefinitely, PES monotonic"). A slicer that doesn't keep PID 256's continuity counter contiguous across slice or loop seams would produce exactly this, with detessify correctly rejecting it.

That is a hypothesis, not a finding. Confirming it means a local repro of the slate path against this exact asset — plugins/prpassemblersrc + libs/detessify_core — not more poking at prod. Want me to set that up? I'd write a failing test first and show you the diff before any production code moves.

Worth noting this also sharpens finding 3: a slate fallback that silently fails is precisely the case the SLI exists to catch, and instead asm_air_slate_seconds_total{media_starved} keeps ticking up as though slate were airing, with asm_channel_state{playing}=1.