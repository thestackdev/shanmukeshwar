---
title: "How to Enable Dark Mode in Google Chrome Liunx"
date: 2023-04-24T13:22:22+05:30
draft: false
cover:
  image: "/blog/chrome-dark-background.png"
  alt: "Chrome dark background"
  relative: false
---

Google Chrome is one of the most popular web browsers in the world. It is available for Windows, macOS, Linux, Android, and iOS. It is a fast, secure, and free web browser. It is also the most customizable web browser. You can customize the appearance of Google Chrome by changing the theme, font, and colors. You can also change the appearance of Google Chrome by enabling dark mode. In this article, we will show you how to enable Google Chrome dark mode in Linux.

## Prerequisites

Before proceeding with the steps to enable dark mode in Google Chrome, ensure that you have the following:

- A Linux operating system
- Google Chrome installed on your system

### Step 1: Copy the Google Chrome desktop file

The first step is to copy the Google Chrome desktop file from the system directory to the user's local directory. This allows the user to modify the file to suit their specific needs. This can include changing the name, icon, and how the application should be launched. By doing so, users can customize their desktop experience and tailor it to their preferences.

To copy the desktop file, open a terminal and run the following command:

```bash
cp /usr/share/applications/google-chrome.desktop ~/.local/share/applications/google-chrome.desktop
```

### Step 2: Modify the Google Chrome desktop file

The next step is to modify the Google Chrome desktop file by adding the `--enable-features=WebUIDarkMode --force-dark-mode` option to the command that launches Google Chrome. This option enables a dark mode for the user interface of the browser.

To modify the desktop file, open a terminal and run the following command:

```bash
sed -i 's;/usr/bin/google-chrome-stable;/usr/bin/google-chrome-stable --enable-features=WebUIDarkMode --force-dark-mode;g' ~/.local/share/applications/google-chrome.desktop
```

### Step 3: Restart Google Chrome

After modifying the desktop file, launch Google Chrome from the applications menu. The browser's user interface should now be in dark mode.

If you want to revert back to the default light mode, simply remove the `--enable-features=WebUIDarkMode --force-dark-mode` option from the Google Chrome desktop file.

## Conclusion

Enabling dark mode in Google Chrome on Linux is a simple process that involves copying the desktop file and modifying it with a command. By following the steps outlined in this article, you can customize your desktop experience and make it easier on the eyes in low-light conditions.
