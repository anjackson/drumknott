# drumknott
An experimental digital [clerk](https://wiki.lspace.org/Rufus_Drumknott).

This is just some notes on an idea for now. Some kind of digital preservation workbench in the browser. A bit like [CyberChef](https://gchq.github.io/CyberChef/), but more a usable 'workbench' for digital preservationers.

## Goal

We need to do basic stuff like scan files, do checksum checks, identify formats. This is often a pain because installing software is a pain. Can we run something useful, just using the browser? There are a lot of things that can run in or be cross-compiled via WASM, so maybe we can make something useful?

## Outline

- We can use the [File System API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_API) (at least in Chrome and Edge) to be given access to a user directory on their local machine.
- We could scan a directory from a [Web Worker](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers), and store information about the files (basic inventory, sizes etc.) in an [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API).
- Other workers could be tasked with running other background tasks:
    - We could run [Siegfried](https://www.itforarchivists.com/siegfried), as per https://siegfried-js.glitch.me/
    - Would run other fancier tools, like `ffprobe` (see below)
    - We could calcuate content hashes, e.g. using https://github.com/satazor/js-spark-md5
- We could export the inventory data as: 
    - [SQLite](https://sqlite.org/wasm/doc/trunk/persistence.md#opfs)
    - [Parquet](https://kylebarron.dev/parquet-wasm/)
    - plain `filepath hash` inventory files a la `md5sum`
    - BagIt?
- We could even run [rclone](https://github.com/rclone/rclone/tree/f491efc85d0bc6c674520331f315b51f060a6b92/fs/rc/js#rclone-as-wasm) and use it to manage data.
- There's also an increasing amount of format-specific tools that may work with WASM, e.g from Rust or Go ecosystems:
    - [pdfcpu ships a WASM release](https://github.com/pdfcpu/pdfcpu)
    - [ffmpeg.wasm](https://github.com/ffmpegwasm/ffmpeg.wasm) exists, so `ffprobe` should be possible. See also [GIF playback via ffmpeg.wasm](https://brunoluiz.net/blog/2022/jan/gif-sane-playback-control-ffmpegwasm/)
    - [ImageWand](https://brunoluiz.net/blog/2022/aug/imagewand-privacy-first-image-conversion-experiment-with-golang-and-wasm/) is an example image toolkit using Go WASM
    - Ruffle (Flash in Rust)

In principle, a `pdfcpu`-based PDF validator would be pretty simple to do, for example.  More generally, probably would need some kind of standard API/wrapping to make it possible to dynamically pull in different format-specific WASM chunks depending on what we find.


