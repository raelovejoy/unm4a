# unm4a

unm4a extracts FLAC-in-M4A files into proper .flac files (no re-encode), preserving attached cover art and metadata.

This is useful when music servers (e.g., Navidrome/Subsonic-family scanners) display track lengths as 0:00 for certain .m4a files that contain FLAC audio.

## What it does

* Detects .m4a files where the first audio stream codec is flac
* Remuxes the audio into .flac using stream copy (lossless, no re-encode)
* Preserves embedded artwork (attached pic)
* Skips non-FLAC .m4a (AAC/ALAC/etc.)

## Install

### Option A: local tool (recommended)

mkdir -p ~/bin
cp -a bin/unm4a ~/bin/unm4a
chmod +x ~/bin/unm4a

Make sure ~/bin is on your PATH (add to ~/.bashrc if needed):

export PATH="$HOME/bin:$PATH"

Reload:

source ~/.bashrc

### Option B: run from repo

./bin/unm4a --help

## Usage

In an album directory (writes to fixed_flac/ inside that directory):

unm4a

Recursive scan:

unm4a -r ~/Music/Albums

Custom output folder name (created inside each directory that has matching files):

unm4a -o fixed_flac .

Write next to sources (no subfolder):

unm4a --inplace .

Remove sources after successful conversion (use carefully):

unm4a --inplace --remove-src .

Dry run:

unm4a -n -r ~/Music/Albums

## Options

* -r, --recursive        Recurse into subdirectories
* -o, --outdir DIR       Output directory (default: fixed_flac). Ignored with --inplace
* -i, --inplace          Write .flac next to source files (no fixed folder)
* --remove-src           Remove source .m4a after successful conversion
* -n, --dry-run          Print what would happen without writing files
* -v, --verbose          Print extra info (codecs, skip reasons)
* -h, --help             Show help
* --version              Print version

## Dependencies

* ffmpeg
* ffprobe

On Fedora/Bluefin:

sudo dnf install ffmpeg

## Notes

* This tool only converts .m4a files that actually contain FLAC audio.
* If your .m4a is AAC or ALAC, unm4a will skip it by design.
* unm4a does not modify your Navidrome database. After converting, rescan your library. If durations are still stuck at 0:00, you may need to clear or rebuild Navidrome's DB depending on your setup.

## License

MIT
