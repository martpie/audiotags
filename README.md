# audiotags2

[![Crate](https://img.shields.io/crates/v/audiotags2.svg)](https://crates.io/crates/audiotags2)
[![Crate](https://img.shields.io/crates/d/audiotags2.svg)](https://crates.io/crates/audiotags2)
[![Crate](https://img.shields.io/crates/l/audiotags2.svg)](https://crates.io/crates/audiotags2)
[![Documentation](https://docs.rs/audiotags2/badge.svg)](https://docs.rs/audiotags2/)

`audiotags2` is a fork of [`audiotags2`](https://crates.io/crates/audiotags2) which is currently unmaintained.

**audiotags2** makes it easier to **parse, convert and write metadata** (a.k.a tag) in audio files from different file types.

This crate aims to provide a unified trait for parsers and writers of different audio file formats. This means that you can parse tags in mp3, flac, and m4a files with a single function: `Tag::default().read_from_path()` and get fields by directly calling `.album()`, `.artist()` on its result. Without this crate, you would otherwise need to learn different APIs in **id3**, **mp4ameta** etc. in order to parse metadata in different file formats.

I'm relatively new to Rust and programming in general, and this is my first attempt to make a non-trivial crate. If you see anything that you think isn't right, just say it!

## Examples

Examples can be found in the [docs](https://docs.rs/audiotags2).

## Performance

Using **audiotags2** incurs a little overhead due to vtables if you want to guess the metadata format (from file extension). Apart from this the performance is almost the same as directly calling function provided by those 'specialized' crates. (It is possible to use **audiotags2** _without_ dynamic dispatch, in which case you need to specify the tag type but benefit from speed improvement).

No copies will be made if you only need to read and write metadata of one format. If you want to convert between tags, copying is unavoidable no matter if you use **audiotags2** or use getters and setters provided by specialized libraries. **audiotags2** is not making additional unnecessary copies.

Theoretically it is possible to achieve zero-copy conversions if all parsers can parse into a unified struct. However, this is going to be a lot of work. I might be able to implement them, but it will be no sooner than the Christmas vacation.

## Supported Formats

| File Fomat    | Metadata Format       | backend                                                     |
| ------------- | --------------------- | ----------------------------------------------------------- |
| `mp3`         | id3v2.4               | [**id3**](https://github.com/polyfloyd/rust-id3)            |
| `m4a/mp4/...` | MPEG-4 audio metadata | [**mp4ameta**](https://github.com/Saecki/rust-mp4ameta)     |
| `flac`        | Vorbis comment        | [**metaflac**](https://github.com/jameshurst/rust-metaflac) |
