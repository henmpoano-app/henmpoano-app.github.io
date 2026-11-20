# HEN MPOANO

One installable Android app that opens both Hɛn Mpoano tools:

- **MEL**: monitoring, evaluation & learning
- **Staff Hub**: weekly reports, leave, vehicles, actions, notices, directory

Both tools keep running from Google Apps Script exactly as they do today. This folder is the
installable "shell" around them: a home screen, a bottom menu, the app icon, and what Android
needs in order to install it.

## Step 1: Allow Staff Hub to open inside the app (one-line change)

MEL already allows this. Staff Hub doesn't yet, so it shows a blank screen inside the app until you make this change.

1. Open the **Staff Hub** Apps Script project, then open `Code.gs`.
2. Find `function doGet()` and add the `.setXFrameOptionsMode(...)` line so it reads:

   ```js
   function doGet() {
     return HtmlService.createTemplateFromFile('Index')
       .evaluate()
       .setTitle(APP_NAME)
       .addMetaTag('viewport', 'width=device-width, initial-scale=1')
       .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL);
   }
   ```
3. Save the file, then go to **Deploy → Manage deployments**, click the pencil ✏️ on the existing deployment,
   set **Version** to **New version**, and click **Deploy**.
   Editing the existing deployment this way keeps the same web-app URL. Don't use "New deployment", because that creates a new URL.

## Step 2: Put the app online with GitHub Pages (free)

1. Sign in at github.com and create a new **public** repository, for example `hen-mpoano`.
2. Click **Add file → Upload files** and drag in **everything in this folder**: `index.html`,
   `manifest.webmanifest`, `sw.js`, `README.md` and the icon `.png` files. Then click **Commit changes**.
3. Go to **Settings → Pages**. Under *Build and deployment*, choose **Deploy from a branch**,
   pick branch **main** and folder **/(root)**, then click **Save**.
4. After a minute or two, the page shows your app's address, for example
   `https://YOUR-USERNAME.github.io/hen-mpoano/`. This is the link to share with staff.

## Step 3: Install on Android

1. Open the link in **Chrome** on the phone.
2. Tap the **Install** button on the home screen, or use Chrome's ⋮ menu → **Install app** (on some phones it says
   **Add to Home screen**).
3. HEN MPOANO now appears with the other apps and opens full screen, with no browser bar.
   Long-pressing the icon gives shortcuts straight to MEL or Staff Hub.

## Good to know

- **Sign-in:** each section still has its own sign-in (MEL: team code + password; Staff Hub: email + PIN).
  People stay signed in while they switch between the sections.
- **Updates:** changes you deploy to MEL or Staff Hub appear in the app straight away. Nothing needs reinstalling.
- **New web-app URL?** If either tool is ever redeployed with a new URL, change it in the `APPS` list near
  the bottom of `index.html` and upload that file again.
- **Changing the shell itself:** after editing `index.html` or the icon files, change `VERSION` in `sw.js`
  (e.g. `hm-v2`) so phones pick up the new version.
- **Offline:** the app opens offline, but MEL and Staff Hub need internet to load their data.
- **Google's banner:** the line "This application was created by a Google Apps Script user" is added by
  Google to every Apps Script app and can't be removed from inside the app.
