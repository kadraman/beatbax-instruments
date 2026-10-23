# beatbax-instruments

Reusable BeatBax instrument kits for **Game Boy** and **NES**.

Files are plain `.ins` kits (`inst`, optional `subpat` / `effect`). Every published file is **self-contained** — remote kits **cannot nest `import`** (BeatBax remote-import security). Prefer leaf files, or one concatenated `kit.ins`.

## Layout

```text
gameboy/
  melodic.ins    # pulse leads, harmony, pads, sweep
  wave.ins       # wavetables and song basses
  drums.ins      # noise kit with macros / subpat specialties
  programs.ins   # subpat + duty_env / arp_env programs
  kit.ins        # concatenated Game Boy kit (no nested imports)
nes/
  melodic.ins    # pulse + triangle (+ macros)
  drums.ins      # noise by period + vol_env
  dmc.ins        # type=dmc → engine @nes/* samples
  kit.ins        # concatenated NES kit
melodic.ins      # compatibility copy of gameboy/melodic.ins
percussion.ins   # compatibility copy of gameboy/drums.ins
gameboy/smoke.bax # local smoke (import "local:kit.ins")
nes/smoke.bax     # local smoke (import "local:kit.ins")
```

## Import examples

```bax
chip gameboy
import "github:kadraman/beatbax-instruments/main/gameboy/kit.ins"
# or leaf files:
# import "github:kadraman/beatbax-instruments/main/gameboy/melodic.ins"
# import "github:kadraman/beatbax-instruments/main/gameboy/drums.ins"

chip nes
import "github:kadraman/beatbax-instruments/main/nes/kit.ins"
```

Legacy paths (still valid):

```bax
import "github:kadraman/beatbax-instruments/main/melodic.ins"
import "github:kadraman/beatbax-instruments/main/percussion.ins"
```

Local smoke (from this repo, no `..` segments):

```bax
import "local:kit.ins"   # when the .bax sits next to kit.ins
```

From BeatBax repo root:

```bash
npm run cli -- export wav instruments/gameboy/smoke.bax tmp/gb_kit_smoke.wav
npm run cli -- export uge instruments/gameboy/smoke.bax tmp/gb_kit_smoke.uge
npm run cli -- export wav instruments/nes/smoke.bax tmp/nes_kit_smoke.wav
```

## Naming

| Chip | Melodic | Drums / DMC |
|------|---------|-------------|
| Game Boy | `gb_*` prefix | short names (`kick`, `snare`, `hat` / `hihat`, …) |
| NES | `nes_*` prefix | short noise names; DMC `*_dmc` |

Song-local `inst` definitions override imported names (last wins).

## Chip features

**Game Boy**

- Hardware `env=`, Pulse1 `sweep=`
- Noise: always set `gb:width`, `uge_note=`, plus `vol_env` / `pitch_env` or a native `subpat`
- Software macros: `vol_env`, `pitch_env`, `duty_env`, `arp_env`
- Native `subpat` for jump/halt/timbre/fx programs (wins over macros on that instrument)

**NES**

- Macros: `vol_env`, `duty_env`, `pitch_env`, `arp_env` (loop with `|index`)
- Noise: `noise_mode`, `noise_period`, often `note=C5` for named hits
- DMC: `dmc_sample="@nes/…"`, `dmc_rate`, `dmc_loop` — samples are **bundled in BeatBax**, not this repo

## License

GPLv3 — see [LICENSE](LICENSE).
