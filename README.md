# Samsung Odyssey G8 Annoying Game Bar Overlay Fix
How to disable the annoying overlay popping up EVERY SINGLE TIME THE SCREEN IS TURNED ON


[Solution](https://www.reddit.com/r/ultrawidemasterrace/comments/14wmdvi/deleted_by_user/jrj6bdy/?context=3)
>You can disable gamebar auto launch via service menu, under "FMS" options.
>
>https://imgur.com/a/bccnBQs
>
>For service menu access, check here: https://www.reddit.com/r/ultrawidemasterrace/comments/12es0ov/how_to_enter_hidden_service_menu_on_samsung_oled/

That's it, if you have a way to access the service menu, you can disable it there. Under the "FMS" section there is an entry called something like "autolaunch game bar" or something similar, with a few options, I've picked the first one which I think said "disabled" or "not supported" or "none". I don't know, I don't want to check again because I have factory reset my screen and I don't want to connect it to the internet again.
Apply that setting, get out of the menu as far as you can, power off, power on, re-enable game mode if necessary (otherwise you don't get 144hz), the bars should stop popping up.

I didn't have a way to enter the service menu - it requires buying a remote that (omg!) actually has buttons and isn't the stylish useless one that comes with the screen, or an Android phone with an IR blaster to pretend to be that remote.
There's another way to get into the service menu, using [commands sent via Home Assistant](https://www.reddit.com/r/ultrawidemasterrace/comments/18j1j2q/service_menu_on_samsung_oleg_g8_with_home/), but I don't have Home Assistant and it felt a bit too much to install it.

So Python to the rescue I guess, if HA can do it, there must be an API. And in fact there is one and it's easy to use.
1. Install https://github.com/xchwarze/samsung-tv-ws-api
2. Replace `192.168.x.x` with your screen's IP (connect it to your LAN if you haven't) and run:
  ```python3
  import sys
  import os
  import time
  from samsungtvws import SamsungTVWS
  
  # Autosave token to file
  sys.path.append('../')
  token_file = os.path.dirname(os.path.realpath(__file__)) + '/tv-token.txt'
  tv = SamsungTVWS(host='192.168.x.x', port=8002, token_file=token_file)
  
  info = tv.rest_device_info()
  
  print(info)
  
  tv.send_key("KEY_INFO")
  time.sleep(0.3)
  tv.send_key("KEY_FACTORY")
  ```
3. The screen will ask to allow the remote connection, allow it
4. You should now be in the factory menu, where you can disable that shit.
5. Check that your screen is working as intended, you might need to re-enable game mode.
6. (Optional) Disconnect your device from the network or factory reset it, the service-settings will stick, so nothing remotely will be able to undo this.

# RANT

Someone at Samsung decided that it was a good idea to make Game Bar overlay pop up EVERY - SINGLE - TIME - THE - SCREEN - IS - SWITCHED - ON.
This one:
![image](https://github.com/mainde/samsung-odyssey-G8-game-bar-overlay-fix/assets/14027750/42fc070b-992b-45d9-8ee6-d5949d004026)

I don't care about that stupid game bar, I never have, I want my computer screen to operate in the way that I've set it to operate, I don't need to be reminded of its settings. It's a colossal waste of time having to sit through those few seconds of nonsense EVERY GODDAMN TIME I TURN ON MY SCREEN.
As if that wasn't bad enough, that stupid and annoying game bar overlay is followed by YET ANOTHER USELESS STUPID AND ANNOYING POP UP reminding the user that they can access some quick settings or some other USELESS CRAP by doing some random unintuitive voodoo ritual on the useless almost button-less remote.
![image](https://github.com/mainde/samsung-odyssey-G8-game-bar-overlay-fix/assets/14027750/7316782a-461e-40d9-bacc-ac01b3e02e5a)

Relevant [YouTube Video](https://www.youtube.com/watch?v=jP2v4d6Nofw)

YES! I have owned this PIECE OF JUNK(*) for ALMOST TWO YEARS but PLEASE, REMIND ME EVERY SINGLE TIME THAT I CAN PRESS SOME BUTTONS TO DO SOMETHING THAT I DON'T CARE ABOUT AND I HAVE NEVER CARED ABOUT AND I WILL NEVER CARE ABOUT!

_(*) I actually quite enjoy the hardware, it would be an mazing product if it wasn't for the asinine UX._

There are COUNTLESS post on the internet from people HATING THIS, people have returned their screens over this. It is insane that it hasn't been addressed!
I and many others have politely provided feedback to Samsung support many times to get this shit sorted, the only responses I got were "have you tried factory resetting your device and disabling the feature that makes your screen work at the expected framerate?" and "press back on the remote to skip through the screens".

My eternal gratitude goes to the engineers that left the setting in the service menu, they knew what was up.
