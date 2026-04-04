# Linux and Brawl modding: A simple guide
This repository exists to outline the steps needed to get BrawlCrate running on Linux, **without VMs or dual booting**, you'll be running this stuff right on Linux with the help of a handy tool called Wine. While the steps are fairly simple, I had trouble actually finding what I needed to do and struggled with BrawlCrate for a while, so I figured I'd share my results to make it more straightforward for future users wishing to abandon Windows but keep BrawlCrate working. This repository will also include premade .desktop files for other tools as well as icons. For me, BrawlCrate has been the single biggest pain-point when I uninstalled Windows, but it's looking like it'll finally be smooth sailing from here on out. And as it turns out, all it took to fix everything was just a few commands!

It should be noted that these resources are for **standard Wine**, not for Bottles, Lutris, etc. I chose to make this guide without the use of Wine helpers, however the steps in this guide should be applicable to any of them, but I won't be covering them at this time. In the future, I may consider making a script to complement this guide but for now there is none; I saw no need because the steps are very simple. Additionally, this guide assumes very basic Linux knowledge, such as what a "home" directory is. (and understanding of what `~/` means in a file path) If you just installed Linux, it is highly advised you make sure you know what each step means before proceeding.

Of course, you'll need [Wine](https://gitlab.winehq.org/wine/wine/-/wikis/Download) and [Winetricks](https://github.com/winetricks/winetricks).

**This guide does not provide any Brawl Modding software, just resources for setting them up. The Brawl tools for this guide can be downloaded here:**
- [BrawlCrate](https://github.com/soopercool101/BrawlCrate)
- [PSA Compressor](https://ux.getuploader.com/iclpxnohokanko1/download/154)

# BrawlCrate setup
## Creating a new Wineprefix and installing .NET 4.8
Run `WINEPREFIX=~/.wine-brawl winetricks --unattended dotnet48` in your terminal to create a new Wine prefix and install .NET 4.8. This also removes Mono, it will take a few minutes. For whatever reason, this changes Wine's Windows version to Windows 7. As BrawlCrate disables plugins if the Windows version is at or below 7, you'll need to change it back to Windows 10 or above. You can do that using `WINEPREFIX=~/.wine-brawl winetricks win10`.

<sub>I've chosen to create a new Wineprefix since there's a chance doing this may interfere with other .NET or Mono applications on the same prefix. You can remove the `WINEPREFIX=~/.wine-brawl` part if you want to do this to your standard `.wine` directory, I chose to keep it separate to avoid potential configuration poisoning. We use .NET instead of Mono, because at the moment, Mono is not usable for BrawlCrate because [a bug](https://github.com/wine-mono/wine-mono/issues/221) causes Mono to ignore Wine's theming settings and BrawlCrate's winforms end up being unreadable. If Mono fixes this issue, I will update this guide accordingly. I hope they do fix it as this will drastically simplify the process.</sub>

## First-time running
If you already have a BrawlCrate folder from your previous Windows install, you can simply copy it wherever you like, and skip this step.
Otherwise if this is your first time running BrawlCrate, or you'd like to start over, download the BrawlCrate exe from the [releases page](https://github.com/soopercool101/BrawlCrate/releases) and save it wherever you'd like it to be. Open a terminal in that folder (typically done by right clicking in your preferred file manager and selecting the option to open the terminal, or using the `cd` command in an already-open terminal) and run `WINEPREFIX=~/.wine-brawl wine BrawlCrate.v0.42h1.x86.exe`, editing the .exe name if its name is different. Don't forget to change (or remove) the WINEPREFIX line if you want it in a different location than `~/.wine-brawl`.
After the self-extracting archive finishes, you may proceed to the next step.

**IMPORTANT:** If you choose to run BrawlCrate manually to try it out before proceeding with the next steps, *DO NOT* use the `File Associations` tab in the settings menu. Wine's way of handling this is quite messy and it's much cleaner to use the premade files I've created. While it works fine, it may look a bit weird. Follow the steps below for a more integrated file associations experience.

## .desktop shortcuts, file associations
On this repository, click the green `<Code>` button and click "Download ZIP", extract it anywhere you want on your PC. Navigate to the `applications` folder and open the files with a text editor. Replace `/your/path/here` with the path to the folder that tool is stored in. For example, mine is `~/Documents/BrawlModding/Tools/BrawlCrate`. Note that it is a path to the folder, not a path to the .exe; the `Exec=` line directly below it will find the executable, assuming it has the correct name.

Copy the `icons`, `applications` and `mime` folders and paste them into `~/.local/share`. You should get the option to write into some folders. Then, run `update-mime-database ~/.local/share/mime && update-desktop-database ~/.local/share/applications` to refresh the MIME types and .desktop cache. This adds the icons to your system and makes the programs discoverable when searching for them, as well as making them pinnable to any taskbar equivalent you have.

<sub>I've included a variety of different icons for BrawlCrate. `brawlcrate-canary` is the default, but you can see the others I've added by looking in any of the sized icon folders yourself and choosing one of a different name, and editing your downloaded `BrawlCrate.desktop` file. If you want the files to have a different icon, you can navigate to the `mime` folder in the package you downloaded (or in `~/.local/share` if you're reading this after already having copied them) and change all instances of `brawl-file` in `<generic-icon name="brawl-file"/>` to another icon name.</sub>

## Done!
It was that simple! Now BrawlCrate should be running for you! To get the .desktop files to start working, you may have to refresh the desktop environment or log out and back in, depends on your desktop. It should be noted that unfortunately, there is no way to force a .desktop file's icon to use what the application provides, the .desktop-provided icon always takes precedent. The only way around this is not having a .desktop file at all, but that can be cumbersome and inconvenient.

Furthermore, I'm not done, I have a few suggestions to improve user experience...

# Optional tweaks
## Dark Theme
Using Wine's theming, we can actually make BrawlCrate as well as any other Wine apps that use the system theme have a dark mode applied to them. First, go [here](https://gist.github.com/Zeinok/ceaf6ff204792dde0ae31e0199d89398), click `Raw` on the `wine-breeze-dark.reg` code block, and save the page as `wine-breeze-dark.reg`. You can save the page with `CTRL+S`. Then, with a terminal in the directory you saved the file in, run `WINEPREFIX=~/.wine-brawl wine regedit wine-breeze-dark.reg`. Now if you open BrawlCrate or any application under your wine prefix, you'll notice it's dark, and easy on the eyes!

## Linked Directories
By default, Wine's file explorer is quite barebones, you may notice it takes quite a lot of navigating to get where you've saved BrawlCrate at. There's also seemingly no way to add folder shortcuts to its quick access panel on the left. It can be quite cumbersome to work with, but we can get around this. Firstly, run `WINEPREFIX=~/.wine-brawl winecfg` to open the wineprefix's configuration. Navigate to the "Desktop Integration" tab. From there you can link up your respective folders on your home directory to the ones on Wine's simulated Windows desktop. You may also navigate to the "Drives" tab and add any folder you'd like as a drive letter. Now after this change you'll notice clicking the documents folder on the quick access panel will take you straight to your own documents folder instead of an empty one, and if you open "My Computer" those drive letter shortcuts you added should be there.

## Discord Rich Presence
Download [rpc-bridge](https://github.com/EnderIce2/rpc-bridge) from its releases page. Open a terminal that's in the directory, and run it with `WINEPREFIX=~/.wine-brawl bridge.exe`. Click "Install".
By default, Wine does not ship the necessary libraries to allow Linux Discord clients to listen to RPC requests. So while BrawlCrate will talk to Wine for its Rich Presence status, Wine won't relay that status to Linux. After running that script, you should see that now when you open BrawlCrate with your wine prefix with Rich Presence enabled, it now shows up in your Discord status. Do keep in mind that if you chose to follow this guide for a sandboxed Wine environment like Bottles or Lutris (especially if they're installed from flatpak) you may need to follow extra steps to get the status to work.

# Final comments
If this guide is updated, you should be able to simply follow the updated steps again, replacing all file overwrite prompts and such. If you wish to start over, delete `~/.wine-brawl` and follow the guide from the very beginning.

## Any questions?
I'd like to help make a place where talking about Brawl modding on Linux feels more at home! Use my [Discussion Tab](https://github.com/Roadhog360/BrawlToolsSetupLinux/discussions) if you have any questions, or suggestions about this guide, or generally for any discussion relating to Brawl modding with Wine. Read the pinned announcement for more info on what I consider "on-topic".

## Credits
- High-resolution PSA Compressor icon remake by Roadhog360
  - Distribution of that icon is allowed without credit, and if the PSA Compressor author sees this, they are free to include it as part of PSA Compressor itself.
- All icons whose names start with "brawlcrate" included in this repo can be found in BrawlCrate's source repo.
- Author of the BrawlBox logo is unknown, presumably one of the original authors before soopercool101.
- Author of brawl-files icon is [RapBattleEditor0510 on DeviantArt](https://www.deviantart.com/rapbattleeditor0510/art/Logos-Super-Smash-Bros-Logo-Icons-737799238).
- All programs mentioned in this guide belong to their respective authors unless explicitly mentioned otherwise. Please refer to the credits pages in any downloads linked for more information about the original authors.
