# ffmpeg-lgpl

A small, static, **LGPL** ffmpeg for Windows, macOS and Linux, built from pinned sources in CI. It is made for
the desktop app of [Sermon Studio](https://github.com/gunnaringe/sermon-studio), which builds it in, but it is
a plain ffmpeg anyone can use.

| Target | Built on |
|---|---|
| `linux-amd64`, `linux-arm64` | Alpine (musl), fully static |
| `windows-amd64` | mingw-w64 on Linux, static |
| `darwin-arm64`, `darwin-amd64` | macOS 11 and later |

## What is in it

- **ffmpeg 9.0.2** with all of its own decoders, demuxers, muxers and filters, LAME 3.100 (MP3), Opus 1.5.2,
  zlib 1.3.1 and whisper.cpp 1.9.5 (the `whisper` filter: speech to text, on the CPU; the models are not included). The versions and SHA-256 of every source are in [`versions`](versions).
- **LGPL:** built without `--enable-gpl` and `--enable-nonfree`. Nothing is taken from the build machine
  (`--disable-autodetect`), and there is no network or devices apart from lavfi.
- ffprobe is built too, for the tests, but not published.

## How it is checked

[`.github/workflows/build.yml`](.github/workflows/build.yml) builds every target on a machine of that kind and
then, on each system:

1. [`check.sh`](check.sh): LGPL, and the encoders, filters and muxers Sermon Studio uses are there.
2. All of Sermon Studio's Go tests, with this build as ffmpeg and ffprobe.

On `main` it publishes the release named in `versions` (`RELEASE`) once: the ffmpeg binaries, the exact
sources, `LICENSES.txt` and `SHA256SUMS`. A new build (another version or other options) needs a new `RELEASE`.

## Build it yourself

```bash
./build.sh linux-amd64        # in Alpine: see the workflow for the packages (cmake too); out/ffmpeg-linux-amd64
./check.sh out/ffmpeg-linux-amd64
```

## Licences

The scripts here are Apache 2.0 ([LICENSE](LICENSE)). What they build is not: ffmpeg is LGPL 2.1 or later,
LAME LGPL 2 or later, Opus BSD, zlib under the zlib licence and whisper.cpp MIT. Each release has them in `LICENSES.txt`, with the
exact sources.
