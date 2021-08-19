---
title: "Gomu Terminal Music Player"
date: 2021-08-19T12:30:55+08:00
tags: ["project-showcase", "music-player", "TUI"]
---

![screenshot-of-gomu](https://res.cloudinary.com/hiremap/image/upload/v1629353143/gomu_rhjsxi.png)

As I was learning [Go](https://golang.org/) programming language, I developed a
software that I been yearning for when I was a child due to the limitations of
most music players nowadays. Go programming language or "golang" is a statically
typed, compiled programming language designed at Google by Robert Griesemer, Rob
Pike, and Ken Thompson. The language itself is easy to learn and you can get the
hang of it fairly quick.

Gomu or **Go** **Mu**sic player is an open-source standalone TUI program. It is
intended to be used inside of the terminal and it is keyboard driven which means
that you will be using your keyboard the most for navigating through the
application.  The keybindings are mostly inspired from vim.

{{< figure \
  src="https://res.cloudinary.com/hiremap/image/upload/v1629353144/gomu-keybind_binfgn.png" \
  width="200" \
  alt="gomu-keybinding" \
  caption="Gomu default keybindings" >}}

The application is able to download audio from youtube and extracts the audio
from it in order to save space in your hardware. Not only that, if the
downloaded video/audio from youtube has closed caption, it will be able to
extract that and display it as lyric. Of course, if the video has missing close
caption you would also be able to fetch the lyric from this
[website](https://www.rentanadviser.com/en/subtitles/subtitles4songs.aspx) by
using `1/2` keybindings depending on the song language (1 is for english and 2
is for chinese)

{{< figure \
  src="https://res.cloudinary.com/hiremap/image/upload/v1629353142/gomu-subtitle_sxsosc.png" \
  width="600" \
  alt="gomu-subtitle" >}}

Quitting gomu and re-open it will cause it to resume for the recently played
song from the queue. The queue is persistent upon closing so you won't have to
manually re-add your songs back to queue. The songs are organized according to
their layout on the filesystem. By default, gomu will search song in `~/Music`
directory. You can always change this in the config `~/.config/gomu/config`

Did I tell you that it has embedded programming language? It uses anko a
golang like scripting language. This scripting language is used to script some
things like adding event listener when the song changes and call `notify-send`
to show notification about the currently played song. It is also used to change
or add new keybindings.

If you want to try out gomu for yourself, you can go to this
[repository](https://github.com/issadarkthing/gomu). Feel free to contribute
😄
