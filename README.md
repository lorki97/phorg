phorg
===============================================================================

NOTE: This is a "heavily" modified fork from [xandkar/phorg](https://github.com/xandkar/phorg).

NOTE: Version 1.0.0 does not indicate stability. I changed it from the initial 0.17.4 to emphasize **INCOMPATIBILITY** with the original phorg version!

Idempotent photo/video file organizer.

NOTE: Both status badges are still pointing to xandkar/phorg.

[![test status](https://github.com/xandkar/phorg/actions/workflows/test.yml/badge.svg)](https://github.com/xandkar/phorg/actions)
[![dependencies status](https://deps.rs/repo/github/xandkar/phorg/status.svg)](https://deps.rs/repo/github/xandkar/phorg)

Overview
-------------------------------------------------------------------------------

Given a `<src>` and `<dst>` directories:

1. finds photo/video files in `<src>`
2. fetches their [Exif](https://en.wikipedia.org/wiki/Exif) data
3. computes their hash digests
4. moves/copies them into
   `<dst>/<year>/<original filename>[.<ext>]`
   where:
    - date and time are extracted from Exif metadata, from whichever of the
      following tags is found first, tried in order:
      - `DateTimeOriginal`
      - `DateTimeCreated`
      - `CreateDate`
      - `DateCreated`
      - `Datecreate`
      - `CreationDate`
      - `TrackCreateDate`
5. optionally, you can (manually) add semantically-named subdirectories
   underneath the `<year>` directory and (manually) move the media files into
   them, these subdirectories will then be preserved on subsequent
   reprocessings, i.e. when this `<dst>` is later used as `<src>`

Example
-------------------------------------------------------------------------------

(note the semantic subdirectory in 2024)

```sh
$ phorg /mnt/usb-drive $dst move
$ cd $dst
$ tree .
.
├── 2023
│   ├── PXL_20230606_172719293.jpg
│   ├── PXL_20230730_164735244.jpg
│   └── PXL_20231011_121556478.mp4
└── 2024
    ├── PXL_20240723_092144500.MP.jpg
    ├── PXL_20240723_092512679.MP.jpg
    └── SomeSortedImages
        ├── PXL_20240722_175510147.MP.jpg
        └── PXL_20240722_180451574.MP.jpg
```

Install
-------------------------------------------------------------------------------

0. Ensure a Rust `1.75.0`+ toolchain is installed: <https://www.rust-lang.org/tools/install>
1. `cargo install phorg`
2. Ensure `~/.cargo/bin/` is in your `PATH`
3. `phorg help`

Dependencies
-------------------------------------------------------------------------------

### Hard

Only the Rust tools mentioned above, everything else will be handled by `cargo`.

### Soft

`exiftool`, which is used as a fallback whenever we fail to extract the needed
metadata ourselves. This fallback **can be disabled** via CLI.

Absence of `exiftool` in `PATH` will cause errors which will be logged, but
**will not disrupt execution** and the files we could not read enough data
about will just be skipped.
