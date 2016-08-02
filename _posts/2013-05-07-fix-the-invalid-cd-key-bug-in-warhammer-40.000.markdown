---
  layout: post
  title: "Fix the invalid CD key bug in Warhammer 40.000"
  categories: archive
---

The anthology edition contains Warhammer 40K - Dawn of War and Winter Assault and Dark Crusade. Installing Dawn of War and Winter Assault went just fine but I ran into some trouble with Dark Crusade. Dark Crusade is a expansion but they've made it into a stand alone game so you can play it without having Dawn of War or Winter Assault installed (or owned). However, if you wish to play the races from Dawn of War and Winter Assault you have to enter the CD-keys from those games.

When I tried to enter these CD-keys I got the error message saying "Your CD-key seems to be invalid. Please try again." even though I have bought the games and the keys are valid. I Googled it and found a lot of people had the same problem (on the Windows side as well) but the only solution they had was to re-install it. I did not feel like re-installing it because it's a pain in the ass and you have to switch between 6 CDs.

I felt that there could be something messy in the Windows Registry Editor since they recommended a re-install so I started searching through it and found the strings which caused this problem. It seems as the Winter Assault and Dawn of War keys for Dark Crusade is located in:

```
HKEY\_LOCAL\_MACHINE/Software/THQ/Dawn of War - Dark Crusade
```

However, the same strings can also be found in:

```
HKEY\_LOCAL\_MACHINE/Software/THQ/Dawn of War
```

and it looks like the game checks these strings as well.

For some reason these strings just contained zeros so what you have to do is to change these strings to your CD-keys in the "Dawn Of War"-folder (not "Dawn of War - Dark Crusade"!). The CD-key string for Dawn of War is named CDKEY (16 chars) and the Winter Assault is named CDKEY_WXP (20 chars). I have also read that some received a 16 chars long CD-key for Winter Assault. This seems to be a printing error and cant be fixed this way.

Start the game again after you've entered your CD-keys (with the "-" between them). You will be prompted for you CD-keys in the game once more but they should report as valid. You can enter them directly in to you "Dawn of War - Dark Crusade" registry folder as well. The Dawn of War string is named `W40KCDKEY` and the Winter Assault is named `WXPCDKEY` (and the CDKEY-string in this folder is for the Dark Crusade game itself). I have heard that the Soul Storm has a similar problem but since I haven't bought it yet I cant tell you how to fix it. My guess is that it is pretty much the same deal.

###Update:

For Windows 7 64-bit: 

```
HKEY\_LOCAL\_MACHINE/Software/Wow6432Node/THQ/
```