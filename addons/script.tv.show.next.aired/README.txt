
How to use this addon in your skin:


I) Startup.xml:
RunScript(script.tv.show.next.aired,silent=True)

The script will scan your library and tries to fetch next aired info for every show.
There is no need to specify an alarm -- the script will will run a background update at regular intervals.

For shows that are airing today, the script will set the window properties listed below.

Window(Home).Property(NextAired.%d.*):
Label               (tv show name)
Thumb               (tv show icon)
AirTime             (eg. 'Wednesday, Thursday: 9:00 pm')
Path                (tv show path)
Library             (eg. videodb://2/2/1/ or videodb://tvshows/titles/1/)
Status              (eg. 'New Series'/'Returning Series'/'Cancelled/Ended')
StatusID            (ID of the status)
Network             (name of the tv network that's airing the show)
Started             (airdate of the first episode, eg. 09/24/07, 'Mon, Sep 24, 2007', etc.)
Classification      (type of show, eg. Reality, Mini-Series, etc. Data is also in Genre.)
Genre               (genre of the show)
Premiered           (year the first episode was aired, eg. '1999')
Country             (production country of the tv show, eg. 'USA')
Runtime             (duration of the episode in minutes)
Fanart              (tv show fanart)
Today               (will return 'True' if the show is aired today, otherwise 'False')
NextDate            (date the next episode will be aired)
NextDay             ("nice" localized format for NextDate, eg. "Wed, Jun 11" or "Mon, Jan 26, 2015")
NextTitle           (name of the next episode)
NextNumber          (season/episode number of the next episode, eg. '04x01')
NextEpisodeNumber   (episode number of the next episode, eg. '04')
NextSeasonNumber    (season number of the next episode, eg. '01')
LatestDate          (date the last episode was aired)
LatestDay           ("nice" localized format for LatestDate, eg. "Wed, Jun 11" or "Mon, Jan 26, 2015")
LatestTitle         (name of the last episode)
LatestNumber        (season/episode number of the last episode)
LatestEpisodeNumber (episode number of the last episode)
LatestSeasonNumber  (season number of the last episode)
AirDay              (day(s) of the week the show is aired, eg 'Tuesday')
ShortTime           (time the show is aired, eg. "20:00" or "8:00 pm")
SecondWeek          (1 == show is in the second week of the Monday-week Guide, otherwise 0)
Art(poster)         (tv show poster)
Art(banner)         (tv show banner)
Art(fanart)         (tv show fanart)
Art(landscape)      (tv show landscape - artwork downloader required)
Art(clearlogo)      (tv show logo - artwork downloader required)
Art(clearart)       (tv show clearart - artwork downloader required)
Art(characterart)   (tv show characterart - artwork downloader required)

Status IDs:
0 - Returning Series
1 - Cancelled/Ended
2 - TBD/On The Bubble
4 - New Series
6 - Final Season
-1 - Undefined

---

Window(Home).Property(NextAired.*):
Total               (number of running shows)
TodayTotal          (number of shows aired today)
TodayShow           (list of shows aired today)


II) MyVideoNav.xml:

Running one of these commands in your skin will provide you with per-show information:
    RunScript(script.tv.show.next.aired,backend=True)

    RunScript(script.tv.show.next.aired,tvshowtitle=The TvShowTitle Show Name)

The first tells the script to run in the background and provide next aired info for the focussed listitem.
The second should be run once for every show-name change.

The infolabels listed above are available, using this format:

Window(Home).Property(NextAired.*)

Use !IsEmpty(Window(Home).Property(NextAired.NextDate)) as a visible condition!

example code:
<control type="group">
	<visible>!IsEmpty(Window(Home).Property(NextAired.NextDate))</visible>
	<control type="label">
		<posx>0</posx>
		<posy>0</posy>
		<width>800</width>
		<height>20</height>
		<label>$INFO[Window(Home).Property(NextAired.NextTitle)]</label>
	</control>
	<control type="label">
		<posx>0</posx>
		<posy>20</posy>
		<width>800</width>
		<height>20</height>
		<label>$INFO[Window(Home).Property(NextAired.NextDate)]</label>
	</control>
</control>


III) If you run the script without any options (or if it's started by the user),
the script will provide a TV Guide window.

This window is fully skinnable -- see script-NextAired-TVGuide.xml and
script-NextAired-TVGuide2.xml (the latter is the today-week guide).

A list of required IDs in script-NextAired-TVGuide.xml, which is used if
the user has selected the traditional, Monday-week guide:

200 - container / shows airing on Monday
201 - container / shows airing on Tuesday
202 - container / shows airing on Wednesday
203 - container / shows airing on Thursday
204 - container / shows airing on Friday
205 - container / shows airing on Saturday
206 - container / shows airing on Sunday
8 - in case all the containers above are empty, we set focus to this ID
(which is typically a settings-button of some kind).

If the user chooses to include more than 7 upcoming days (including today), then
episodes from the next week are included after this week's episodes for each
day.

A list of required IDs in script-NextAired-TVGuide2.xml, which is used if
the user has selected the new, Today-week guide:

200 - container / shows aired Yesterday
201 - container / shows airing Today
202 - container / shows airing Today+1
203 - container / shows airing Today+2
204 - container / shows airing Today+3
205 - container / shows airing Today+4
206 - container / shows airing Today+5
207 - container / shows airing Today+6
208 - container / shows airing Today+7
209 - container / shows airing Today+8
210 - container / shows airing Today+9
211 - container / shows airing Today+10
212 - container / shows airing Today+11
213 - container / shows airing Today+12
214 - container / shows airing Today+13
215 - container / shows airing Today+14
8 - in case all the containers above are empty, we set focus to this ID.

If the user chooses to include fewer than the full 15 upcoming days (including
today) and/or to disable Yesterday, then the skin should be prepared to hide
the days that aren't enabled (the *.Weekday, *.Wday, and *.Date values below
will be unset for any disabled containers).

Various Window(home) vars that we provide (some are more useful in just one of
the 2 xml files, but all are always set):

For the following date values, the user can choose between the traditional
number values (e.g. 12/31/99) and a nicer format (e.g. "Sun, Dec 31").  The
format is consistent across values, allowing you to (for example) string-
compare a show's NextDate value against the TodayDate (or YesterdayDate) to
substitute the string for "Today" (or Yesterday).

Today's date and a localized word for "Today":
    Window(home).Property(NextAired.TodayDate)
    Window(home).Property(NextAired.Today)
Yesterday's date and a localized word for "Tomorrow":
    Window(home).Property(NextAired.TomorrowDate)
    Window(home).Property(NextAired.Tomorrow)
Yesterday's date and a localized word for "Yesterday":
    Window(home).Property(NextAired.YesterdayDate)
    Window(home).Property(NextAired.Yesterday)

The date for the lists (Monday==1, Sunday==7):
    Window(home).Property(NextAired.1.Date)
    ...
    Window(home).Property(NextAired.7.Date)

For the following container values, only the ones that are enabled by the
user will have a set value.  For instance, if the user has selected 7-days
in a today-week grid w/o yesterday, only properties 201..207 would be set.

The day-of-the-week name for each container (not abbreviated).
    Window(home).Property(NextAired.200.Weekday)
    ...
    Window(home).Property(NextAired.215.Weekday)

The abbreviated day-of-the-week name for each container:
    Window(home).Property(NextAired.200.Wday)
    ...
    Window(home).Property(NextAired.215.Wday)

The date for each container in a "nice" format, with just the month name and
day num (e.g. "Feb 14" & "14 Feb" are 2 typical localized formats):
    Window(home).Property(NextAired.200.Date)
    ...
    Window(home).Property(NextAired.215.Date)

A list of available infolabels:
    ListItem.Label          (tv show name)
    ListItem.Thumb          (tv show thumb)
    ListItem.Property(*)    (see above)

Totals are available using the window properties listed above.

Thumb type selected by the user (0=poster, 1=banner, 2=logo):
    Window(home).Property(TVGuide.ThumbType)

Indicator for background fanart setting (1=enabled, empty if disabled):
    Window(home).Property(TVGuide.BackgroundFanart)

Indicator for 16:9-thumbs setting (1=enabled, empty if disabled):
    Window(home).Property(TVGuide.PreviewThumbs)

All other IDs and properties in the default script window are optional and not required by the script.


IV) To force an update of the nextaired database ahead of its next scheduled time:
RunScript(script.tv.show.next.aired,force=True)

To force an update as well as reset all the existing data (forcing a fresh scan of everything) use the reset option:
RunScript(script.tv.show.next.aired,reset=True)

The force update and reset options are also available in the addon settings.

V) To force the update of a single show (re-reading all its data), it can be added via a button like this one:

<control type="button" id="550">
<label>$LOCALIZE[24069] $LOCALIZE[4]</label>
<include>DialogVideoInfoButton</include>
<onclick>RunScript(script.tv.show.next.aired,updateshow=$INFO[ListItem.Label])</onclick>
<visible>Container.Content(tvshows)</visible>
</control>
