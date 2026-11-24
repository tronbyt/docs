---
title: Advanced
icon: lucide/graduation-cap
---
# Advanced Features

To write really great apps, it's helpful to understand how Tronbyt
devices function, and how they interact with the backend (where
community apps run). There are a few advanced features that can
influence certain aspects of these interactions, which we'll cover
here.

## Device-side caching

Tronbyt devices maintain a cache of the most recently received graphic
for each of its installed apps. This serves several purposes. First of
all, it allows the device to continue to display graphics during
intermittent network failures, including when the device is forced to
reconnect to the backend. Second, it eliminates the need to re-ship
graphics that have already reached the device, reducing network
usage.

This mode of operation does come with a significant drawback: if new
graphics are unable to reach a device, the device will continue to
display older graphics even when these are out of date. An
illustrative example of this being problematic is the clock app. If
some sort of glitch is preventing up-to-date graphics to reach a
device, the clock will effectively be running behind.

To counter this, Tronbyt apps can optionally specify a `max_age` on
their `Root` widget. When present, this field instructs Tronbyt devices
to discard the graphic when a certain amount of time has
passed. During normal operation, the backend will ship out replacement
graphics before this happens, but when that fails it is in some cases
better to display nothing at all, than to display stale data that the
user will assume is correct.

## App cycle speed
!!! note

    `show_full_animation` only works when the app is deployed to the Tronbyt servers.
    Please see [Publishing Apps](publishing-apps.md).

Most users have multiple apps installed on their Tronbyts. By default,
each will be displayed for about 15 seconds, although this _app cycle
speed_ is user configurable. For apps that generate static, single
frame graphics, this is straightforward: the graphic is displayed for
exactly as long as the user has requested. For apps that generate
animations, things get a little bit more tricky.

If an animation fits within the allotted time window, it will be
displayed as many times as possible, without truncating the
animation. For instance, if an animation is 7 seconds long, and the
app cycle speed allows for 15 seconds per app, then the animation will
be displayed twice for a total of 14 seconds. While a single second
remains of the time window, using it to display a small piece of the
animation a third time will typically result in a jarring experience
for the user. We don't want that.

If an animation exceeds the time window, it will be truncated to
fit. This gives the user a predictable experience, where they know
roughly how long it'll take for the next app to appear. It also
prevents buggy apps from unintentionally hijacking the display for long
periods of time. In some cases, however, this behavior is not ideal.

For instance, an app that displays quotes isn't all that useful if the
quotes get truncated halfway through. While one can argue that shorter
quotes should be used, that isn't always a reasonable solution. For
such cases, the `show_full_animation` field on the `Root` object can
be used to request that the device's app cycle speed is ignored, and
that the animation is shown in full. If the user is bothered by the
longer-than-normal app duration, they'll simply have to uninstall the
app.

It can be tempting to throw this setting on all sorts of apps, but we
would encourage developers to use some restraint. If the app can be
redesigned to fit all its data in a shorter animation, then that may
be preferable. If not, then `show_full_animation` may be the way to
go.

## Asking for a notification sound

Devices with a speaker or buzzer (such as the Tidbyt Gen 2) can play a
notification sound for an app. Users choose the sound and when it
plays in the app's settings on their Tronbyt server. Most triggers,
like *Every time app is displayed* or *Only when display content
changes*, need nothing from the app.

Some apps change their image on every render but only have something
worth a sound now and then. A scores app shows a ticking match clock,
but should only make a noise when a team scores. A calendar app counts
down every minute to when you need to leave, but should only beep at
10 minutes, 5 minutes and "leave now". Only the app knows which
changes matter, so it can ask for the sound itself by passing
`sound_id` to its `Root` widget:

```python
def main(config):
    match = get_match()
    return render.Root(
        sound_id = "match-%s-score-%d-%d" % (match.id, match.home, match.away),
        child = render_score(match),
    )
```

When the user has chosen a sound and set the trigger to **When the app
asks**, the server plays that sound the first time a new `sound_id`
arrives. It remembers the last `sound_id` it played for each
installed app, so returning the same value again on later renders
doesn't repeat the sound. Your app doesn't have to remember what it
has already announced: keep returning `match-123-score-2-1` for the
rest of the match, and the next goal's `match-123-score-2-2` plays.

Things to know:

- Make each `sound_id` name the event, not the time. Something like
  `leave-<event id>-10min` plays once; anything containing the current
  time would play on every render.
- Leave `sound_id` out (or pass `""`) when there's nothing to
  announce. Nothing plays, and the last ID is still remembered.
- The user's chosen sound is played, and the user's mute, night mode,
  dim mode and volume settings still apply. A new `sound_id` that
  arrives while the device is muted is skipped, not saved for later, so
  it can't play after the device is unmuted.
- An app's sounds are at least 30 seconds apart. A request that arrives sooner
  isn't lost: it plays on a later render that still returns it.
- Values longer than 256 characters, or containing control
  characters, are ignored.
- Nothing plays unless the user picks **When the app asks**, so
  describe in your app's README which events make a sound.
- Previews (on the app's settings page, or when rendering with
  `pixlet render`) never play sounds. `pixlet render` logs the
  `sound_id` your app returned, which is handy for testing.

!!! note

    `sound_id` needs a recent Pixlet and Tronbyt server. Older versions
    reject the unknown argument, so the app fails to render on them.
