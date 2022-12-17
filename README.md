# CSS hacks for Firefox

This repository contains some styles to modify appearance of Mozilla Firefox. These stylesheets are mostly self-contained and can be mixed with each other somewhat freely, but there are no promises about compatibility with third-party styles.

In the case that a particular style relies on another style, the fact will be noted at the start of the file that requires so.

Stylesheets in this repository are tested on fedora bspwm. Most of them should also work on other systems, but there may be wrong behavior especially when native widgets such as window title bar or window control buttons are being styled.

# Setup

As an overview, you will make Firefox load two special stylesheets - `userChrome.css` and `userContent.css`. Doing so requires setting a specific preference (see below) and then creating those files inside your Firefox user profile.

The setup is quite straightforward with the exception of how to find the profile folder so pay attention to that.

## Set the pref to load stylesheets

Type `about:config` in the address bar and set the preference `toolkit.legacyUserProfileCustomizations.stylesheets` to `true`

After you set this preference, Firefox will try to load `userChrome.css` and `userContent.css` - but those files don't exist yet so now let's create them.

## Setting up files

### Find the profile folder

First, find your profile folder. While Firefox is running, go to `about:support` and find a `Profile folder` row near the top - there should also be a button labeled "Open folder" next to it. Clicking that button should open the folder in your file manager.

NOTE: On some Firefox versions clicking that button may open the **profiles** folder which houses *all* your profiles. In that case, navigate into the specific folder you wish to modify. `about:support` should still show the correct folder name so refer to that if you need to figure out the what folder you need to open.

The real profile folder should have files like `prefs.js` and `places.sqlite` If you see those two files in the folder, then great! You found the profile folder! Now lets actually create those stylesheet files.

### Creating the stylesheet files

Note: only userChrome.css is mentioned in this section for brevity, but everything regarding that will also apply to userContent.css

Firefox loads `userChrome.css` from `<profileFolder>/chrome/userChrome.css`. That chrome-folder or the stylesheet files do not exist by default.

### Set up files manually

<details>
<summary>Manually copying individual styles directly into userChrome.css is a simple way to do things for better and for worse.</summary>

0. Create a new folder into the profile folder and name it `chrome`
0. Create `userChrome.css` inside that newly created chrome-folder
0. Copy-paste contents of individual .css files from this repository into your userChrome.css file (and save it of course!)
0. If Firefox is running, restart Firefox so that the changes take effect

**Pay attention to the filename** of `userChrome.css` - the file extension must be `.css` and if your file manager is hiding file extensions then you might accidentally create a file named `userChrome.css.txt` and Firefox will not load that.

In the end you should have a folder structure like this:
```
<profile_folder>
|_ chrome
|   |_ userChrome.css
|   |_ userContent.css
|_ extensions
|_ prefs.js
...
all other profile folders and files
...
```

</details>

### Set up files using git

<details>
<summary>Preferred way to do things, since it makes updates easier and makes organizing multiple styles easier.</summary>

Assumes that you have a git client installed, and that you do not already have a chrome folder in your profile.

0. Open a command prompt / console / terminal and `cd` into the profile folder
0. Clone this repository into the profile folder
    * `git clone https://github.com/richardbendli/firefox-config chrome` on command-line
    * This should create a new folder "chrome" into your profile folder with the contents of this repository
    * (**NOTE**: if you already have "chrome" folder, then rename it before cloning. After clone is complete, just copy the *contents* of the old folder into the new chrome folder)
0. (Optional) Make a copy of `userChrome_example.css` and rename the copy to `userChrome.css`
0. `@import` individual style files into your userChrome.css
    * Notice tha any `@import`s must be placed before anything else in whatever file you are using them
    * Check userChrome_example.css for how it uses `@import`
0. If Firefox is running, restart Firefox so that the changes take effect

Afterwards, you can just use `git pull` in the "chrome" folder and it will replace your copies with up-to-date versions. `git pull` won't replace your userChrome.css file so you can safely put your own custom rules into userChrome.css directly and those won't be overwritten when you update.

</details>