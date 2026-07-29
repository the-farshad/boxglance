# boxglance

A single-file Bash dashboard that prints CPU, memory, disk, and GPU stats at a glance.

```
⠙ myhost 14:32:07 up 3h12m   CPU ●●●◉········   28%  load 1.42/16c ●◉···········   9%
MEM RAM ●●●●◉·······  38% 12.1/32.0G free 18.2G | SWAP ◉···········   0% 0.0/8.0G
DSK /        ●●●●●●●◉····  61% 210G/350G  /data    ●●◉·········  18% 90G/500G
TOP CPU python3 96%          MEM chrome 11%
────────────────────────────────────────────────────────────────────────────
GPU Name       T      Util               VRAM                     Pwr    #  Jobs
0   3090       ●●●●●◉·······  45°  ●●●●●●●◉····  62%  ●●●●●◉·······  41% 10.1/24G  210W   1  1234:alice:train.py:10.1G
1   3090        ●●◉·········  38°  ◉···········   0%  ◉···········   0% 0.0/24G     22W    0  -
```

## Features

- CPU usage (sampled over a short interval, not an instantaneous snapshot), load average, and uptime
- RAM and swap usage
- Disk usage across one or more mount points
- Top CPU and memory consuming processes
- Per-GPU utilization, temperature, VRAM, power draw, and running compute jobs (NVIDIA only, via `nvidia-smi`)
- Color-coded bars and thresholds (green / yellow / red) for at-a-glance health checks
- A `--watch` mode for a continuously refreshing view

## Requirements

- Bash 4+
- Core utilities: `awk`, `ps`, `df`, `free`, `nproc`
- Optional: `nvidia-smi` for the GPU block (skipped gracefully if not present)
- Optional: `watch` for `--watch` mode

No non-standard dependencies and no installation step beyond making the script executable.

## Install

```sh
curl -fsSLo boxglance https://raw.githubusercontent.com/the-farshad/boxglance/main/boxglance
chmod +x boxglance
./boxglance
```

Or clone the repo and run it directly, optionally placing it on your `PATH`:

```sh
git clone https://github.com/the-farshad/boxglance.git
cd boxglance
chmod +x boxglance
sudo cp boxglance /usr/local/bin/
```

## Usage

```sh
boxglance                 # print one snapshot and exit
boxglance --watch         # refresh every 1s
boxglance --watch 5       # refresh every 5s
boxglance -h              # show help
```

## Configuration

All configuration is via environment variables:

| Variable           | Default                                   | Description                                                        |
|--------------------|--------------------------------------------|----------------------------------------------------------------------|
| `DISKS`            | auto-detected existing common mount points | Space-separated list of mount points to report on                  |
| `BAR_W`            | `12`                                       | Width of progress bars, in characters                               |
| `MAX_JOBS_PER_GPU` | `2`                                        | Max GPU jobs listed per device before collapsing the rest into `+N` |
| `CPU_SAMPLE`       | `0.10`                                     | Sampling interval, in seconds, used to measure CPU usage             |

Example:

```sh
DISKS="/ /mnt/data /mnt/backup" BAR_W=20 ./boxglance
```

## License

MIT — see [LICENSE](LICENSE).
