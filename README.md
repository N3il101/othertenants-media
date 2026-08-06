# othertenants-media

Rendered reels, their covers, and their captions. Nothing here is written by
hand — every file arrives as a GitHub Release asset, uploaded by
`.github/workflows/build.yml` in the pipeline repository.

## Why the files live here and not in the code repository

A rendered reel is 20–30 MB of H.264. Committing two of those a day would put
roughly 15 GB a year into a git history that can never be made smaller, to
store artifacts that are regenerable from the script and the footage manifest.
Release assets sit outside the object database, so cloning the code stays cheap.

## Why this repository is public

Instagram is handed a URL and fetches the video **itself**, unauthenticated. A
private media repository produces a container that sits in `IN_PROGRESS`, then
fails with `ERROR` several minutes later, with a message that never mentions
permissions. `scripts/instagram-doctor.js` checks this specifically, because it
is the failure that looks least like its cause.

Nothing secret is ever uploaded here. The reels are the thing being published.

## Layout

One release per post, tagged `media-<idea-id>`, holding:

| asset | what it is |
|---|---|
| `<idea-id>.mp4` | the reel, 1080×1920, H.264 + AAC |
| `<idea-id>-cover.jpg` | the cover still, shape name over the animal |
| `<idea-id>.txt` | the caption the post ships with |

The Discord approval message links straight to the `.mp4`, which is also the
file to save if you are posting it to TikTok by hand — TikTok's Content Posting
API restricts un-audited clients to private-only posts, so that one platform
stays manual on purpose.

## Why this file exists at all

A GitHub Release creates a git tag, and a tag must point at a commit. While this
repository was empty, every upload failed with `422 Repository is empty` — after
the pipeline had already paid for a cloned voice and a full render. This commit
is the fix.
