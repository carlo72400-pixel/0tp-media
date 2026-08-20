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
