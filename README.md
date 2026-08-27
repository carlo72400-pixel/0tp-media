# 0TP MEDIA

Full quality photos and video for **0FF THE PRINT**.

This is not a website. It is a bucket. The files live on **releases**, one release
per event, and the galleries at
[carlo72400-pixel.github.io/0fftheprint/events](https://carlo72400-pixel.github.io/0fftheprint/events/)
point at them.

## Why it exists

GitHub release assets have no total size limit and no bandwidth limit, and take
files up to 2 GiB. Repo files are capped at 100 MB and count against the site's
size and traffic.

Keeping the media here means the site repo only carries small grid thumbnails,
so the galleries can ship 2560px frames and 1080p video instead of 1600px
frames and 720p clips that got deleted for being too big.

## ⛔ VIDEO CANNOT LIVE ON RELEASES. Photos can.

Found 2026-08-27. GitHub serves **every** release asset as

    content-type: application/octet-stream
    content-disposition: attachment

whatever the file actually is. `hero.jpg` gets the identical headers.

An `<img>` does not care, it sniffs the bytes, which is why every photo on
every gallery has always worked and hid this for months. A `<video>` DOES care:
**iOS Safari refuses to play a source typed octet-stream and marked as an
attachment**, so every clip on the site was dead on every iPhone while playing
fine in desktop Chrome. There is no per-asset content-type control on that
endpoint.

So the split is now:

* **Photos stay on releases.** No size limit, no bandwidth limit, and `<img>`
  does not care about the headers.
* **Video is committed to this repo under `video/<slug>/` and served by GitHub
  Pages**, which types by extension and returns a real `video/mp4`. Clips are
  re-encoded for the web first (720p or 608x1080, ~1.6 Mbps, `+faststart`), so
  a night is tens of megabytes rather than hundreds.

That keeps the SITE repo lean, which is the reason this repo exists, while
giving video a host that will actually play it.

## How to add to it

Do not upload here by hand. Run the builder in the site repo, it does the whole
thing:

```
/usr/bin/python3 newevent.py "/path/to/03 Graded Photos" \
    --venue "Paper Tiger" --title "Blade Rave" --date 2026-08-14 \
    --videos "/path/to/04 Graded Video" --limit 44
```

Each release is tagged with the event slug, e.g. `2026-08-14-blade-rave`.
Re-running an event deletes and recreates its release, so a rebuild never
leaves stale frames behind.
