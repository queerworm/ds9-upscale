# DS9 Upscale Workflow

## Mission
Deep Space Nine is an excellent show and my favorite Star Trek series by far,
but the video quality available for it leaves much to be desired.

DS9 is available on pretty much every streaming service under the sun (in the
US, it's currently on Netflix, Amazon Prime, Hulu, and CBS All Access), which
makes it really easy to just start watching (I originally watched all of the 
pre-Discovery shows on Netflix), but the video quality is absolutely horrendous.

The original DVDs are *much* better, but they're still only in standard
definition. However, CBS is unlikely to release an official remaster like they
did with TOS and TNG, so it's what we've been stuck with (the sales of those
apparently didn't justify their cost, and DS9 is both far less popular and more
difficult to remaster, due to it using a lot more CGI than TNG).

Until now. Enter Topaz Video Enhance AI, a piece of software that uses machine
learning to upscale video. I'm generally a skeptic when it comes to promises of
ML's power (I prefer old-school programmer-written software with explainable
results, thank-you-very-much), but after seeing it work it's magic on a few
clips, I was impressed. It's by no means perfect, and there's some videos that
it just isn't good with (my experiments with upscaling some old football games
were a complete bust), but it works well for DS9 most of the time.

I've now set about upscaling all of DS9 using this software.

Unfortunately, while Topaz is impressive, it's quirks and the quirks of the
show combine in some weird ways to prevent it from being a one-button perfect
solution that I can just dump a pile of videos in and get HD DS9 goodness out.

My goal with this project is not to make a perfect version of DS9 (frankly, only
CBS could do that), but instead to make something that is better than what
currently exists. While the show could benefit from someone going through
episode-by-episode or even scene-by-scene to find the best settings that work
for each clip and spot-fix things that Topaz gets wrong, that's far more work
than I'm willing to put in.

With that in mind, this workflow is one-size-fits-all, so I'm applying it
the same way across all episodes. It's also only semi-automated, as Topaz
unfortunately lacks a scriptable interface. Since Topaz is by far the biggest
bottleneck (it takes > 6 hours to upscale a single episode), there's no real
point in me trying to combine all the post-processing steps into a single shell
command, since I have to wait hours between each Topaz run anyway.

With that in mind, here's the process I'm using.

## Hardware/Software Requirements
- a recent-ish Nvidia graphics card (I used a GTX 1660 Super)
  - Topaz can also run off of an Intel integrated GPU, or even directly off of
    a CPU, but it takes so much longer (by a factor of 5 or 10, respectively)
    that it's not a good idea
- a computer running Windows 10
  - Topaz technically supports macOS too, but since macOS doesn't support Nvidia
    GPUs, it's not really feasible to use it for anything but short clips.
  - [Full System Requirements][]
- [Topaz Video Enhance AI][] (paid software, though there is a free trial)
  - This guide assumes you're using v1.2.2 or v1.2.3 (the latest stable version
    at the time of writing). There's a beta version of v1.3.0 out right now, but
    it lacks support for exporting to PNG files, which this guide depends on.
- WSL with [Fish Shell][] installed
  - Note: This isn't strictly necessary, but I prefer Fish to Bash or
    Powershell, so my scripts here will be written using it. They're simple
    enough that you could always port them to your shell and language of choice.
- [ffmpeg][] and [MKVToolNix][] (both free-of-charge)
- lots of free disk space
  - The final upscaled videos will usually be <2 GB per episode, but you'll want
    at least 100 GB of free space per episode you have in flight, so if you want
    to queue up several episodes at once, you'll need lots of space.
  - I worked off of an NVMe SSD and then moved my completed episodes off to an
    external HDD (where I also kept source files until I was ready to use them),
    but the tools used are slow enough that you're unlikely to be bottlenecked
    by working entirely off of an HDD.

[Full System Requirements]: https://help.topazlabs.com/hc/en-us/articles/360039302251-Video-Enhance-AI-System-Requirements
[Topaz Video Enhance AI]: https://topazlabs.com/video-enhance-ai/
[Fish Shell]: https://fishshell.com/
[ffmpeg]: https://www.ffmpeg.org/download.html
[MKVToolNix]: https://mkvtoolnix.download/downloads.html

## Source Files
This workflow assumes you already have access to a high-quality rip of the DS9
DVDs in a format that Topaz will accept. My computer doesn't have an optical
drive, and it's not worth it to me to get one just to rip the DVDs myself, so
I'm using DVD rips from someone known as "JCH", which I downloaded from RARBG.
I've found them to be very high quality, and I'm happy with the results of
upscaling them. These rips also embed chapters and subtitles in the files
themselves, which means I don't have to track them down separately.

If you rip the DVDs yourself, or use a different existing rip, you'll want to
make sure that you preserve the original variable framerate source. Most scenes
are ~24 fps, but some scenes (mostly those involving CGI effects) are ~30fps.
You'll also want to make sure that your rips are deinterlaced, as Topaz doesn't
support interlaced files.

## Limitations

As I said above, Topaz is not perfect. I've found that it improves the quality
from the DVD rips most of the time, and even when it doesn't make things better,
it rarely makes them worse. That being said, here are some issues that you may
notice when viewing the completed upscales:

- The upscaling process can occassionally add a small amount of jitter (or make
  existing jitter a bit more noticable). I usually don't notice it when viewing
  episodes normally, but it is there when I look for it.

- The more detail in the source material, the more Topaz has to work with, so
  close-ups usually look really good, but wider shots and background details
  won't improve much, and may occassionally look over-sharpened.

- Topaz can struggle with on-screen text. It does well with titles and credits,
  so those tend to look much better, especially from Season 4 onward (the first
  three seasons used a different font that Topaz doesn't do as well with).
  However, text in the actual scene (such as on PADDs) will usually look the
  same or worse. It should still be readable if it was in the original source,
  but it's not going to pull out any more details there.

- The upscaler can make mistakes sometimes, but most of the time I think there's
  a glitch, I check the original source and it's there as well. Of particular
  note, there are some blinking lights above the main entrance to Quark's that
  have fooled me multiple times into thinking the upscaler was glitching.

## I. Setup
Make sure you've installed Topaz and WSL on Windows, and that you've installed
the Fish Shell within your WSL environment.

For ffmpeg and MKVToolNix, you can install them either on Windows or in WSL,
though you'll want to make sure that both `ffmpeg`, `mkvextract`, and `mkvmerge`
are all accessible from your Fish path either way.

> I use the Windows versions of these tools from WSL using aliases like:
> `alias ffmpeg='/mnt/c/path/to/ffmpeg.exe'`

Make a working directory on a drive with plenty of free space and copy the first
episode you want to upscale into it.

For the purposes of the commands here, we'll use the fourth-season episode
Rejoined, and use Plex's preferred filename format:

```
Star Trek- Deep Space Nine - S04E06 - Rejoined DVD.mkv
```

If you're going through all episodes in order, keep in mind that the first
episode (Emissary) is double-length, so it will take twice the time and disk
space as most other episodes. This is also true of the season 4 premiere (The
Way of the Warrior) and the series finale (What You Leave Behind). 

## II. Upscaling

The big thing that prevents us from just generating an MP4 from Topaz directly
is that it gets confused by variable frame rate files. Topaz sees the 30fps
segments and attempts to encode based on that, even though most scenes are
actually 24fps. This means that the video Topaz generates is 25% faster and
horribly out of sync with the audio.

To get around this, we'll instead have Topaz output a series of PNG files and
then stitch them back together ourselves with ffmpeg.

First, we need to find out how many frames our video actually has, since Topaz
will guess incorrectly and end up doing more work than it needs to.

Add this function to your Fish shell:

```fish
function count-frames -a video
    mkvextract $video timestamps_v2 0:timestamps-$video.txt >/dev/null 2>/dev/null
    echo (math (cut -d" " -f1 (wc -l timestamps-$video.txt | psub)) - 1)
    rm timestamps-$video.txt
end
```

and then run it with `count-frames *Rejoined\ DVD.mkv`. In my case, I got 65684
frames. For a regular 45-minute episode, this shouldn't be significantly
different from that. If it's more in the range of 80K+, it's likely that your
rip was converted to 30fps, which should look stuttery, but if it looks fine to
you, you're welcome to use it (in that case, you could probably skip most of
this and just have Topaz output directly to mp4, since the frame rate wouldn't
be an issue).

Now, open Topaz Video Enhance AI and drag your episode into it.

There are five things we need to do before we start the upscale:

1. Change the A.I. Models option from it's default value of "Gaia-HQ: P, HQ" to
   "Artemis-LQ: P, LQ, MC".
   
> You're welcome to experiment with the different models to see if you like one
> of the others better, but in my experience, Artemis-LQ gives the best results
> on most scenes.

2. The output preset should default to 200%, which I recommend. This will give
   you video that's ~1280x960 (minus cropping), which is as wide as 720p, but
   with a 4:3 aspect ratio. You're welcome to use 400%, which gets you ~QHD
   quality, but the higher the resolution, the longer the upscale will take.

> If you plan on burning to a Bluray (which is a [whole other process][] that I
> can't really recommend), you may want to consider one of the "HD" presets,
> which will burn-in black bars on the sides of the video to make it 16:9 (which
> is necessary for Blurays if you don't want the image stretched), but
> otherwise, you should stick to the original aspect ratio.

[whole other process]: https://2020mirror.com/2016/12/28/adventures-in-blu-ray-authoring/

3. Change "Output Format" from "mpeg4" to "8-bit PNG". Note that "8-bit" here is
   per-channel, so that should be plenty for DS9. You'd only really need the
   "16bit TIFF" option if your source was HDR, and ours is most certainly not.

4. In the bar at the top, Topaz should report that there are >80K frames in the
   video, which we know from our frametimes script is wrong. You should set the
   end frame to the count that we found above (~65K), with a small buffer added
   so that we know we got them all.

> If you use the seek bar to select around what should be the last frame, the
> preview will show something ~80% through the episode. This is wrong, and Topaz
> will start repeating sections of the episode when actually encoding, so you
> can ignore the preview now. If you're paranoid, you could just tell Topaz to
> upscale the whole video, but doing so will add at least an hour or so to your
> processing time.

> If Topaz actually reports the frames as the same number our script found, then
> your source is probably a constant frame rate. As above, assuming you're okay
> with any stutters you see in the source video, this whole process shouldn't
> actually be necessary.

5. Check that the width and height of the frames Topaz will be outputting are
   both even. The encoding settings we'll be using don't support odd dimensions.
   If either is not, switch from the 200% preset to Custom and then subtract
   one pixel from the odd dimension(s) to make it even. The source video should
   always have even dimensions already, but Topaz converting the anamorphic
   pixels of the source material into square pixels can sometimes result in this
   issue. So far, the only episode I've had this issue with is Emissary.

> Even dimensions aren't strictly necessary if you're using ffmpeg's default
> yuv444p chroma subsampling setting, but many video players can only handle the
> more standard yuv420p for H.264, so that's what we'll be using. It's also
> possible to use Topaz's default dimensions here and then crop the odd pixel
> during the ffmpeg encode (which I did when I only noticed the issue after
> upscaling Emissary), but doing so significantly slows down the encode, so it's
> better to do it here.

Now that our settings are fixed, we can click on the play button in the bottom
left to start the upscale. On my machine, each episode takes a little over 6
hours, but this can vary based on your hardware, machine usage, and any
settings you set differently.

You can also queue up multiple episodes at a time if you'd like, but keep in
mind that you'll need 100 GB of free space per episode if you won't be there to
do the post-processing and delete the PNGs you're done with.

## III. Turning Frames into Video

Once Topaz finishes, we should have a folder with >65K PNG files that we want
to turn into an actual video. Open the folder and check that all the frames are
there.

Assuming you left a small buffer when setting the end frame in Topaz, you should
see the sequence of frames end with the Paramount logo fading out, then several
frames of black, and then some frames from the middle of the episode.

Given the frame count you got in Step II, verify that the frame with that
number is one of the black frames between Paramount and the middle of the
episode.

Now that we have our frames, we need to combine them back into an actual video.

We'll use the following fish function (note: this uses the `count-frames`
function from above, so make sure it's still available).

```fish
function rebuild-ds9-video -a original output
    set frames (count-frames original)
    set folder (string replace .mkv _ $original)
    cd {$folder}*
    ffmpeg -i %06d.png -c:v libx264 -profile:v high -pix_fmt yuv420p -preset medium -crf 18 -vframes $frames ../$output
    cd ../
end
```

which you can run with

```fish
rebuild-ds9-video *Rejoined\ DVD.mkv raw-upscale.mkv
```

You may wish to use different settings for H.264, or even a different codec
entirely. The `-crf` flag controls the quality while the `-preset` flag controls
the compression. With these settings, each normal-length episode is ~1.5 GB for
me. Given the whole point of this project is to improve the video quality, I
wouldn't recommend increasing the CRF much (which lowers quality), but anything
lower than 18 would likely result in diminishing returns.

If you want smaller video sizes without sacrificing quality, you could use a
slower preset to compress the video stream more. With my settings, this encode
takes ~25 minutes (though that's with Topaz running on the next episode at the
same time with higher priority).

See [this page][] for more details on ffmpeg's settings for H.264.

[this page]: https://trac.ffmpeg.org/wiki/Encode/H.264

This command turns our folder full of PNG files into a video, but there's two
issues we need to fix: (1) there's no audio, and (2) the framerate is actually
a constant 25fps, so the 24fps seconds are slightly sped up and the 30fps
sections are slowed down. We'll fix both of these issues in the next step.

## IV. Building the Final MKV

To build our final video, we'll use MKVToolNix. All of this can be done in its
GUI if you prefer, but I use the CLI to make it scriptable.

Regardless of which method you prefer, there are 3 things we want to do here:

1. Get the frame timestamps from the original video.
2. Apply those frame timings to our upscaled video from the previous step.
3. Add the audio, subtitles, and chapters from the original video into our
   upscaled MKV.

I use the following Fish function:

```fish
function make-ds9-mkv -a original upscale
    set timestamps timestamps-$original.txt
    mkvextract $original timestamps_v2 0:$timestamps
    set parts (string split ' - ' $original)
    set title (string replace ' DVD.mkv' '' $parts[3])
    set output (string replace DVD HD $original)
    mkvmerge --title $title -o $output --timestamps 0:$timestamps $upscale -D $original
    rm $timestamps
end
```

This function first uses `mkvextract` to extract the frame timestamps from the
original video and then it builds a new MKV file with following settings:

1. The upscaled video, but with the timestamps we extracted from the original
2. All non-video channels of the original MKV
3. The episode's title, parsed from the original filename
4. The new MKV's filename is the same as the original, but with "DVD" replaced
   with "HD"

You can then make the final MKV file with:

```
make-ds9-mkv *Rejoined\ DVD.mkv raw-upscale.mkv
```

## IV. Making Clips

This isn't strictly relevant if you're just upscaling episodes for your personal
use, but I figured it made sense to post the settings I use for encoding the
clips that I post on [Tumblr][].

For most clips, I just use the following Fish function:

```fish
function make-clip -a source start end out
    ffmpeg -i $source -ss $start -to $end -c:v libx264 -profile:v high -crf 18 -c:a aac -af "channelmap=channel_layout=5.1" $out
end
```

I'm using the same video settings as above, but I am re-encoding instead of
cutting the existing video stream because that led to audio sync issues. I also
convert the audio from AC3 to AAC because browsers lack support for AC3 audio.

After encoding the clip, if it's larger than 100MB (Tumblr's file size limit),
I'll then encode into a WebM with VP9 video and Opus audio.

Tumblr does re-encode the clips into smaller files after I upload them, but it
doesn't seem like much (if any) visual quality is lost. The episode MKV files
(which only went through one encode after upscaling) will probably look better
than the clips in some cases though.

[Tumblr]: https://queerspaceworm.tumblr.com

## Conclusion

That's it! Repeat this process for any and all DS9 episodes you want to watch in
HD. I haven't tried this process on Voyager, but I don't see any reason why it
would work any differently. TNG and TOS already got official HD remasters, which
are far better than anything Topaz could do, and Enterprise was released in HD
originally.

I'm working through the whole series using this workflow myself. I started with
Season 4 (since it has a lot of my favorite standalone episodes) and I'm now
working through the rest starting from the beginning.

See [here][status] for the current status of my upscales. If I redo any upscales
with different settings or an updated version of Topaz, I'll note that there as
well.

[status]: https://github.com/queerworm/ds9-upscale/blob/master/status.md
