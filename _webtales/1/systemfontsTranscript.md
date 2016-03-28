# S1E2 - System Fonts - DONE

Improving the readability of our website is one of our primary jobs. And since simple text makes up about 95% of most websites, what better way to optimize readability than to make your text clear and highly legible. One of the best strategies to achieve that is by using the same battle tested fonts used by the end user’s operating system; letting them read your text in a friendly familiar fontface.


I’m talking about being able to harness the same default fonts used by the the operating system in places like menus, alert boxes, login screens etc. We want to be able to display text in the device’s own native _system font_ — like  “San Francisco” for OS X and iOS; “Roboto” for Android, “Ubuntu Font” for Ubuntu and so on.

## Goal
Our goal in this episode is to figure out a practical way to use the native system font inside our webpage, regardless of which browser the end user is on.


It isn’t the same as setting `font-family` to `serif` or `sans-serif`. That just uses the browser’s default font families, which even on the same device, varies from browser to browser. An end users can pick their own browser default font as well. So we’ll need a better way to achieve our goal than just using `serif` and `sans-serif`.
%% SLIDE: defaults for each browser displayed in a table


## Quick Dirty Way
The first method is to just list the system fonts for all operating systems; starting with the most uncommon ones.
%%SHOW font family set to all in order
The idea is that since these fonts are checked for availability in the order they’re listed. If it doesn’t exist, we know it’s not the right system font for this device. The next one gets looked up then and so on and so forth. Hopefully, one will eventually be found on the device and we _assume_  that that’s the right system font and just go with it. After all, OS X wouldn’t have the Ubuntu Font now would it? 
It actually might! This approach has a few drawbacks —

1. Some fonts, like the Ubuntu font, are distributed for free and there is a likelihood that someone using OS X or Android has it installed on their device already. Since we listed the Ubuntu font ahead of the OS X system font, on account of it’s uncommonness … you can see this method gets us the wrong result. It should’ve been San Fransisco, but no! ++whatfont++
2. In order to stay up to date, you, as the web developer take on the endless burden of keeping track of the latest system font for every single operating system out there. If there happens to be an operating system system font change  and you don’t update the CSS on time, you risk displaying text with an outdated font family that might or might not even exist in that operating system anymore, possibly resulting in the incorrect font again.
3. In OS X and iOS, Apple recommends not using explicit labels to refer to the system font “San Fransisco”, like we did here. Yes it works now, but there is no guarantee it will in the future. Chrome helps out a wee bit here by providing the `BlinkMacSystemFont` value that always refers to the OS X system font. Only works within Chrome of course.


## font property
The second method uses `font` — a shorthand property capable of setting a whole slew of typographic settings with one single line. 

%% SLIDE illustrating all of these
%% - font-style
%% - font-variant
%% - font-weight
%% - font-size 
%% - line-height
%% - font-family

It comes with a few terms and conditions …
%% POINT Mozillas Note section
%% SLIDE with multiple entries for font: variable

And when you don’t explicitly set a value here, that value does not inherit from the parent, like you’d expect it to; it is set to an “initial” value instead.
%%  SLIDE
%% - font-style: normal
%% - font-variant: normal
%% - font-weight: normal
%% - font-stretch: normal
%% - font-size: medium
%% - line-height: normal
%% - font-family: depends on user agent

Something not quite well-known about `font` is that it accepts these _shortcut_ keywords. 
''caption icon menu message-box small-caption status-bar

When set to, say `menu`, the browser automatically sets font’s values to match the font of an operating system menu text — like the family, size, weight, line-height etc. They all get set for you. 
%% SLIDE
%% Ubuntu -> Ubuntu Font.
%% Yosemite -> Helvetica.
%% El Capitan -> San Francisco. .SFNSText-Regular
%% Windows -> Segoe UI.

My computer is running on OS X El Capitan, so we can expect this text to be displayed in San Francisco and here’s how they end up looking
''<p style="font: caption">hello</p>
''compare that to san fransisco font sample
''SHOW for each style by uncommenting
Using these keywords gives us 6 font combinations and it might it seem like we have enough combinations to provide a native web experience, but consider these downsides — 
1. Once you set the font with one of these keywords, altering individual font preferences like size and weight is not allowed. Any attempts to do so will reset the remaining font preferences to their initial values. So you either set your values manually or use one of these shortcut keywords, but not both.
	++SLIDE Initial Values++
2. This method is standards compliant and browser implementation is sortof good on the desktop with a few claims of inconsistencies; but definitely not at all good on mobile platforms like iOS where it’s flat out not implemented yet.
3. This might be a subjective opinion, but I’m not sold on using names such as caption and menu. Just doesn’t seem like the right naming scheme. 

One final point while on this topic, Overall nice guy Firefox implements a couple more keywords to complement the current set. Use them if they make sense; because they only work with Gecko based browsers like Firefox —
%%		-moz-button
%%		-moz-info
%%		-moz-desktop
%%		-moz-dialog (also a color)
%%		-moz-document
%%		-moz-workspace
%%		-moz-window
%%		-moz-list
%%		-moz-pull-down-menu
%%		-moz-field (also a color)


## `system`
The third method is to use the `system` keyword in the `font-family` attribute. 

This is an Apple proposal and as such, currently has support only within their ecosystem; but it does work splendidly on both iOS and OS X. 
''font-family: -apple-system;
As simple as that! 

Since we get to specify the font in the `font-family` this time, feel free to change any other font preferences.
''font-size: 3em;

Apparently, there are ongoing discussions about making `system` a part of the CSS spec, so we’l have to keep your fingers crossed.

e
## Conclusion
So far, you learned three different ways to harness the system font. 
- Manually listing ‘em in font-family
- Using shorthand keywords in the `font` attribute
- Using `system`

__So what is the best, most practical method to use, as of today? __

I’m gonna defer to Marcin from the Medium team on this one. 
He proposes using `system` with the first method — by  placing the `-apple-system` and `BlinkMacSystemFont` values ahead of rest of the  `font-family` list. It’s not perfect; the first method’s drawbacks still apply; but it is the most practical way to achieve our goal of using system fonts in our webpages!

Hope you learned something useful. See you around!
