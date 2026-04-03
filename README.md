# Linux and Brawl modding: A simple guide
This repository exists to outline the steps needed to get BrawlCrate running on Linux, **without VMs or dual booting**, you'll be running this stuff right on Linux with the help of a handy tool called Wine. While the steps are fairly simple, I had trouble actually finding what I needed to do and struggled with BrawlCrate for a while, so I figured I'd share my results to make it more straightforward for future users wishing to abandon Windows but keep BrawlCrate working. This repository will also include premade .desktop files for other tools as well as icons. For me, BrawlCrate has been the single biggest pain-point when I uninstalled Windows, but it's looking like it'll finally be smooth sailing from here on out. And as it turns out, all it took to fix everything was just a few commands!

It should be noted that these resources are for **standard Wine**, not for Bottles, Lutris, etc. I chose to make this guide without the use of Wine helpers, however the steps in this guide should be applicable to any of them, but I won't be covering them at this time. In the future, I may consider making a script to complement this guide but for now there is none; I saw no need because the steps are very simple.

Of course, you'll need [Wine](https://gitlab.winehq.org/wine/wine/-/wikis/Download) and [Winetricks](https://github.com/winetricks/winetricks).

**This guide does not provide any Brawl Modding software, just resources for setting them up. The Brawl tools for this guide can be downloaded here:**
- [BrawlCrate](https://github.com/soopercool101/BrawlCrate)
- [PSA Compressor](https://ux.getuploader.com/iclpxnohokanko1/download/154)

## BrawlCrate setup

### Creating a new Wineprefix and installing .NET 4.8
Run `WINEPREFIX=~/.wine-brawl winetricks --unattended dotnet48` in your terminal to create a new Wine prefix and install .NET 4.8. This also removes Mono, it will take a few minutes.

<sub>I've chosen to create a new Wineprefix since there's a chance doing this may interfere with other .NET or Mono applications on the same prefix. You can remove the `WINEPREFIX=~/.wine-brawl` part if you want to do this to your standard `.wine` directory, I chose to keep it separate to avoid potential configuration poisoning. We use .NET instead of Mono, because at the moment, Mono is not usable for BrawlCrate because [a bug](https://github.com/wine-mono/wine-mono/issues/221) causes Mono to ignore Wine's theming settings and BrawlCrate's winforms end up being unreadable. If Mono fixes this issue, I will update this guide accordingly. I hope they do fix it as this will drastically simplify the process.</sub>

## .desktop shortcuts
On this repository, click the green `<Code>` button and click "Download ZIP", extract it anywhere you want on your PC. Navigate to the `applications` folder and open the files with a text editor. Replace `/your/path/here` with the path to the folder that tool is stored in. For example, mine is `~/Documents/BrawlModding/Tools/BrawlCrate`. Note that it is a path to the folder, not a path to the .exe; the `Exec=` line directly below it will find the executable, assuming it has the correct name.

Copy the `icons` and `applications` folders and paste them into `~/.local/share`. You should get the option to write into some folders. This adds the icons to your system and makes the programs discoverable when searching for them, as well as making them pinnable to any taskbar equivalent you have. I've included a variety of different icons for BrawlCrate. `brawlcrate-canary` is the default, but you can see the others I've added by looking in any of the sized icon folders yourself and choosing one of a different name, and editing your downloaded `BrawlCrate.desktop` file.

## Done!
It was that simple! Now BrawlCrate should be running for you! To get the .desktop files to start working, you may have to refresh the desktop environment or log out and back in, depends on your desktop.

Furthermore, I'm not done, I have a few suggestions to improve user experience...

# Optional tweaks
## Dark Theme
Using Wine's theming, we can actually make BrawlCrate as well as any other Wine apps that use the system theme have a dark mode applied to them. First, go [here](https://gist.github.com/Zeinok/ceaf6ff204792dde0ae31e0199d89398), click `Raw` on the `wine-breeze-dark.reg` code block, and save the page as `wine-breeze-dark.reg`. You can save the page with `CTRL+S`. Then, with a terminal in the directory you saved the file in, run `WINEPREFIX=~/.wine-brawl wine regedit wine-breeze-dark.reg`. Now if you open BrawlCrate or any application under your wine prefix, you'll notice it's dark, and easy on the eyes!
## Linked Directories
By default, Wine's file explorer is quite barebones, you may notice it takes quite a lot of navigating to get where you've saved BrawlCrate at. There's also seemingly no way to add folder shortcuts to its quick access panel on the left. It can be quite cumbersome to work with, but we can get around this. Firstly, run `WINEPREFIX=~/.wine-brawl winecfg` to open the wineprefix's configuration. Navigate to the "Desktop Integration" tab. From there you can link up your respective folders on your home directory to the ones on Wine's simulated Windows desktop. You may also navigate to the "Drives" tab and add any folder you'd like as a drive letter. Now after this change you'll notice clicking the documents folder on the quick access panel will take you straight to your own documents folder instead of an empty one, and if you open "My Computer" those drive letter shortcuts you added should be there.
## Discord Rich Presence
Download [rpc-bridge](https://github.com/EnderIce2/rpc-bridge) from its releases page. Open a terminal that's in the directory, and run it with `WINEPREFIX=~/.wine-brawl bridge.exe`. Click "Install".
By default, Wine does not ship the necessary libraries to allow Linux Discord clients to listen to RPC requests. So while BrawlCrate will talk to Wine for its Rich Presence status, Wine won't relay that status to Linux. After running that script, you should see that now when you open BrawlCrate with your wine prefix with Rich Presence enabled, it now shows up in your Discord status. Do keep in mind that if you chose to follow this guide for a sandboxed Wine environment like Bottles or Lutris (especially if they're installed from flatpak) you may need to follow extra steps to get the status to work.
