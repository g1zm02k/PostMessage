# Good Day!

Here's a quick guide to using Spy++* to find the `PostMessage` code to open the 'About' window of Notepad - but it can be used for a large number of other applications (providing they support it), this includes, but isn't restricted to, sending button presses to media players, opening specific menu items without having to navigate them first, the list goes on...

Simply put, `PostMessage` allows Windows objects to communicate with each other - for example, using the mouse to click on an app menu item basically triggers a message that the app interprets as a uniquely numbered UI item being activated and responds by showing that menu item's window, or similar effect - using `PostMessage` we can send the same message directly, completely bypassing the need for any mouse interaction at all.

*\*I use Microsoft Spy++ as it's literally the only software I could find that still works! The downside is that it only ships with Visual Studio so I don't know what the legal standing is for sharing that software as a stand-alone app (so don't ask me for it). I can say that there's a copy on GitHub (five versions actually) if you know your way around a search engine (v14!)*

With that out of the way, let's get straight in and fire up Notepad and the version of Spy++ relevant to your OS...

# Spy++

![S01](https://user-images.githubusercontent.com/47895215/130149344-8f80d564-1972-45b8-b1dc-a271d78d8d44.png)

Note: *Bear in mind that the toolbar buttons change depending on the active window.*

First, we need to hook Spy++ into Notepad, so click the fifth icon along (window + binoculars) and you'll open the Find Window panel.

![S02](https://user-images.githubusercontent.com/47895215/130149404-8a225f1f-5fa0-4629-9559-d3cc4c1a997a.png)

Click and drag the cross-hair symbol from this panel over Notepad's titlebar and let go.

![S03](https://user-images.githubusercontent.com/47895215/130149617-a8550d87-5aac-4a12-b783-281b98b4c8a3.png)

![S04](https://user-images.githubusercontent.com/47895215/130149634-d63ceacd-5d96-4ffc-b332-f1f3605287ab.png)

Now that we've locked on, make sure 'Messages' is selected on the bottom and click OK.

![S05](https://user-images.githubusercontent.com/47895215/130149673-946f36a2-3c04-4ef5-a675-08334d3e8504.png)

Note: *You might see a lot of stuff going on in this window; don't panic, we're going to sort that now...*

Maximise this 'Messages' window and click on the seventh icon ('Logging Options', to the right of the traffic light, looks like an app window)...

![S06](https://user-images.githubusercontent.com/47895215/130149789-503b8055-d38a-4c9f-8973-7e087cee4cfb.png)

**Message Options:**

Make sure everything in the Windows tab is unchecked except 'Save Settings as Default'.

![S07](https://user-images.githubusercontent.com/47895215/130149825-54d79c13-e2e4-4360-9362-0f00de7d462a.png)

On the Output tab, uncheck everything aside from 'Decoded Message Parameters' and 'Save Settings as Default'.

![S08](https://user-images.githubusercontent.com/47895215/130149861-ef8901c9-3a0a-4dcb-8718-07f7b6968b2e.png)

Finally, on the Messages tab, click 'Clear All' to ensure 0 messages selected shows, then scroll through the 'Messages to View' panel until you get to WM\_COMMAND (\~4/5ths down) and select it, making sure that's the only message selected. Again, tick 'Save Settings as Default' and 'OK' out of there.

![S09](https://user-images.githubusercontent.com/47895215/130149896-4740dd4d-ddf9-4748-aa09-6c5f48356e06.png)

As this might be your first time, if there's anything in the messages window you should clear it now so we can start fresh with the new settings.

![S10](https://user-images.githubusercontent.com/47895215/130150021-d2523e70-5578-4efc-a3e3-bd1cbc3e3a70.png)

# Now we can interact with Notepad!

As soon as you activate Notepad you'll see two lines pop up in Spy++, you can ignore these for now as they're SendMessages and not what we want, you can tell by the second parameter being an 'S' - we're looking for a 'P', for PostMessage of course...

![S11](https://user-images.githubusercontent.com/47895215/130150104-e53ae9dd-c98e-4e7d-b39c-4080d1808857.png)

Click 'Help > About Notepad'...

![S12](https://user-images.githubusercontent.com/47895215/130150142-8f2c2550-dd17-4ca7-b3ce-e8a9780799ea.png)

And there it is!

![S13](https://user-images.githubusercontent.com/47895215/130150337-c10ee666-8689-45f7-ade7-e3d69a223642.png)

There's a line which is noticably different from the rest with a 'P' for the second parameter:

![S14](https://user-images.githubusercontent.com/47895215/130150365-eadca45e-5b6d-4f45-8ed6-a0af62985796.png)

    16CHR_HEX_STRING P WM_COMMAND wNotifyCode:0 (sent from a menu) wID:65
                     ^This was sent by PostMessage!

# Writing the code

The PostMessage parameters that we'll need from this, as listed in the docs, are:

    PostMessage Msg,wParam,lParam,,WinTitle

* `Msg` is WM\_COMMAND but that's not an AHK variable so we need to use its hex code instead: `0x111`.
* `wParam` is the number we got with wID: `65`.
* `lParam` is the number we got with wNotifyCode: `0`.
* `WinTitle` is something you should know already by now or you're reading the wrong guide...

Anyway, let's plug those values in:

    PostMessage 0x111,65,0,,ahk_exe Notepad.exe

Now run it, and... there it is!

*I'm going to have to save the Bluetooth trick itself for another guide as this one has taken it out of me over the last three days* (",)
