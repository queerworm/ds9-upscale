# DS9 Upscale Project

This repo contains a guide on how to upscale [Deep Space Nine][] using
[Topaz Video Enhance AI][] as well as the current status of my upscale.

[Deep Space Nine]: https://en.wikipedia.org/wiki/Star_Trek:_Deep_Space_Nine
[Topaz Video Enhance AI]: https://topazlabs.com/video-enhance-ai/

If you want to perform this upscale yourself, follow [this guide][guide].

[guide]: https://github.com/QueerWorm/ds9-upscale/blob/master/guide.md

If you'd rather just download an existing upscale, magnet links are
available on my Reddit, /u/queerspaceworm.

If you download my upscale, I'd appreciate feedback [here][survey].

[survey]: https://docs.google.com/forms/d/e/1FAIpQLSdyO1X9FKTjwgw9tNnMEYXUZ-F1C0ZL3jH_OmO6e0uL7-PLvg/viewform

I post clips and status updates on [Tumblr][].

[Tumblr]: https://queerspaceworm.tumblr.com

## FAQ

### Why only 960p? Why not 1080p or 4K?

Diminishing returns. The upscaling software takes more than 6 hours to process
each episode as it is. No matter what resolution I choose to output, an AI
upscale will never be as good as a proper remaster by CBS. The higher resolution
I choose to output, the more time, disk space, and bandwidth it takes.

Additionally, AI-based upscaling is fundamentally just software making
educated guesses at details that aren't present in the source material, and the
higher resolution I ask for, the more it has to guess based on the same
information, increasing the frequency with which it guesses wrong in a noticable
way.

Given the limitations of the source material and the software, a 2x upscale hits
the sweet spot for me by noticably improving the visual quality compared to the
DVD source without the costs (in GPU time, disk space, bandwidth, and upscaling
errors) being too high.

If you'd prefer a higher resolution, you're more than welcome to use my guide to
upscale it yourself. Some anecdotal evidence from others suggests that Topaz's
performance for higher resolutions is better when it goes through multiple
passes (e.g. 2x up to 960p, then 2x again up to 1920p), so you could even try
starting from my upscales and then upscaling again to your desired resolution.

### Is this legal?

> Note: I am not a lawyer. This is not legal advice. All of this applies only
> to the United States.

If you're downloading the upscales that I've made, definitely not, but you
probably already knew that.

If you're ripping DVDs you own and upscaling them yourself by following my
guide... it's actually *still* illegal! The DMCA does have an exemption for
making backups of content you own a copy of, but if you need to break DRM to do
it (and, unlike CDs, pretty much all commercially-sold DVDs have DRM), it's
still illegal. This isn't something individuals have been sued over AFAIK though
(though the creators of the software used to break the DRM of DVDs *have* faced
legal action).

As for the morality of it, I doubt even the most cynical media executive would
think that ripping and upscaling DVDs you own for your own private use is
morally wrong.

Upscaling it all yourself requires a significant amount of GPU time and a
license to the software though, so I think that most reasonable people would
agree that torrenting the upscale if you own the DVDs is morally fine.

Personally, I don't really see a huge moral issue with pirating a decades-old
TV show from a giant corporation, especially if you already pay for access to a
streaming service where you could watch it legally (and all the pre-Discovery
Star Trek shows are currently available on Netflix, Hulu, Amazon Prime, and
CBS All Access in the US, so there's a good chance you do).

***tldr: Legal? No. Moral? That's up to you to judge.***

### Is it safe to download this?

If by "safe" you mean "does it have any viruses?", then yes, it's safe.

However, torrenting copyrighted material can get you flagged by your ISP, so you
may want to use a VPN to do so. As I understand it, this flagging process
requires the copyright holder to be monitoring the torrent in question so they
can report you to your ISP, so depending on the popularity of this upscale, and
how much ViacomCBS cares about you downloading it, you might be able to get away
without one. Most ISPs use a three-strikes system for this as well, so if you've
never received a warning before, you could try your luck without one.

> The penalties for torrenting on networks you don't control can be much more
> severe, so unless you want to be fired or expelled, don't torrent on your
> school network without a VPN or on your work network at all.

Using a VPN is the smart choice for this though. When shopping for a VPN, don't
listen to their marketing about how it's necessary to protect your general
online browsing, and you shouldn't enable it for your day-to-day web use.
[This video][video] is a good overview of the problems with how VPNs market
themselves. These sorts of VPNs really only have three types of customers:
paranoid people, pirates, and people who actually need to hide what websites
they're visiting from the person or organization who pays for their internet.
The VPN companies don't want to be seen marketing to pirates, and the latter
demographic is fairly small, so they focus on the paranoia.

[video]: https://www.youtube.com/watch?v=WVDQEoe6ZWY

I connect to a VPN from [PrivateInternetAccess][] when torrenting, and I've
generally found it to be reliable and fast, and it's cheap when you pay for a
year or more in advance. I don't use it for general browsing though, I don't
recommend you do either, unless your threat model is a parent or partner who
controls your internet. You shouldn't trust your ISP, but you *definitely*
shouldn't trust some random VPN company.

[PrivateInternetAccess]: https://www.privateinternetaccess.com/

***tldr: No viruses, but use a VPN to prevent being flagged by your ISP.***

### I found a visual bug in your upscale. Can you fix it?

Maybe. If it doesn't look worse than the DVD source, or if it's unlikely to be
noticed during normal watching, I probably won't bother, but you can file an
issue on this repo and I'll keep an eye out for it if/when I do another upscale
of that episode.

### How's the visual quality compare to the remaster for the documentary?

No contest. This upscale at its best can't compete with the scenes they
remastered for [What We Left Behind][doc]. However, remastering just those
scenes was a huge undertaking, and unless CBS decides they want to do it for the
whole show (unlikely), AI upscales are our best hope at improving on what we've
got.

[doc]: https://en.wikipedia.org/Star_Trek:_Deep_Space_Nine#Documentary_What_We_Left_Behind

### What's the meaning of your username?

I'm a queer woman, as is Jadzia Dax, whose symbiont could also be described as
a worm. Hence, QueerWorm. On Reddit and Tumblr, that name was already taken, so
I'm queerspaceworm instead.

### Can you upscale Voyager?

I haven't tried it myself, but I assume the process I use for DS9 should work
just as well for Voyager. At the moment, I don't have plans to upscale Voyager
myself (I'm just not as interested in rewatching it as I am in DS9), but if I
ever do in the future, I'll mention so here.

If someone else uses my guide to upscale Voyager, let me know and I'll link it
here.

### Can you upscale `$X`?

Probably not. The intersection of "shows that aren't available in HD" and
"shows I care about enough to invest the time into upscaling them" is pretty
small.

One show that did fit this venn diagram is [Avatar: The Last Airbender][], but
you don't need me to upscale it, as it's [already been done][]. Nickelodeon also
did an official HD release, though I've found the fan-made one to be better for
most episodes.

[Avatar: The Last Airbender]: https://en.wikipedia.org/Avatar:_The_Last_Airbender
[already been done]: https://www.reddit.com/r/RemasteringATLA/comments/5hr9w2/atla_remastered_in_1080p/

### Do you have a PayPal I can use to support you?

This project was started purely out of my own interest in seeing Deep Space Nine
in HD and I have a stable job that pays me enough that I can afford to spend
some of it on computer hardware and fancy AI upscaling software.

If you have money that you would have used to pay some random girl on the
internet with a GPU and a lot of free time on her hands, you should instead use
it to [support the protestors fighting for racial justice][donate].

[donate]: https://bailfunds.github.io/
