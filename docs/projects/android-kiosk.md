# Turning an Android Phone into a Kitchen Kiosk

## Overview

I had a drawer full of old devices that I could never quite bring myself to throw away. One of them was an older Xiaomi phone that had already become a small project in itself. I rooted it first so I could remove unnecessary apps, clean up the system, and get more control over how it behaved.

I cover that process separately in [How to Root an Android Phone](#).

Once that was done, I decided to take it a step further and turn the phone into a **kitchen kiosk**—a simple, shared device with a few large buttons for everyday tasks.

My original goal was to build a full smart display, but I had to scale it back when I couldn’t get the phone to connect to an external screen. So I shifted to something simpler and more practical:

- A clean home screen  
- Four large buttons for specific tasks  
- No distractions  
- No accidental taps into random apps  
- A setup simple enough for anyone in the house to use  

This guide walks through the exact steps I followed.

---

## What I Wanted the Device to Do

Before changing anything, I decided exactly what the phone needed to do. I kept the scope narrow on purpose.

The home screen would include four buttons:

1. **Shopping List**  
   Open a list quickly and (ideally) allow adding items by voice.

2. **School Schedule**  
   Show what books my son needs each day—something that currently takes too much time every morning.

3. **Shabbat Times**  
   Display the week’s Shabbat time and eventually trigger music 15 minutes before it starts.

4. **Photos**  
   Open a slideshow.

---

## Step 1: Root and Clean the Phone

The first step was rooting the phone so I could remove manufacturer apps and reduce background activity.

I describe that process in detail in [How to Root an Android Phone](#).

Once rooted, I removed apps I didn’t need. The goal wasn’t to strip the system completely—it was to keep only what supported the dashboard.

### What I removed

- Xiaomi bloatware  
- Unused Google apps  
- Preinstalled apps I wouldn’t use  
- Extra utilities that added clutter  

### What I kept

- Core Android system apps  
- Google Play Services  
- Play Store  
- MacroDroid  
- A launcher  
- Apps needed for the dashboard features  

### Why this mattered

This step made the phone feel faster and simpler. More importantly, it helped shift the device from “old phone” to “dedicated tool.”

---

## Step 2: Choose a Home Screen Setup

I tested a few launcher options before settling on one that fit the project.

### What I tried

- **MIUI Launcher** — Too restrictive and still felt like a regular phone  
- **Niagara Launcher** — Clean, but too list-based for shared use  
- **Lawnchair** — Flexible, but still felt like a standard Android setup  
- **Square Home** — Best fit for a tile-based dashboard  

### What I chose

I chose **Square Home** because it made it easy to create a simple, large-button interface.

### How I installed it

1. Downloaded it from the Play Store  
2. Opened **Settings → Home Screen**  
3. Set **Square Home** as the default launcher  

After that, the phone booted directly into the new layout instead of the standard Xiaomi home screen.

---

## Step 3: Build the Four-Button Layout

With Square Home set, I built the main interface.

### Goal

A home screen with only four large tiles:

- Shopping List  
- School Schedule  
- Shabbat Times  
- Photos  

### Setup process

1. Cleared everything from the home screen  
2. Added tiles one by one:  
   - Tap **+**  
   - Choose: Application / Shortcut / Launcher Action / Widget  
3. Resized each tile to make it easy to tap  
4. Labeled each tile clearly  
5. Locked the screen to landscape:  
   **Home Options → Behavior and UI → Screen Orientation → Landscape**

---

## Step 4: Add Automation

Some behavior needed automation, so I installed **MacroDroid**.

I used it to:

- Launch the dashboard on boot  
- Keep behavior consistent  
- Prepare for time-based actions  

### Boot automation

I wanted the dashboard to appear automatically after a restart.

### Setup

1. Open **MacroDroid**  
2. Create a new macro  
3. Set **Trigger** → Device Boot  
4. Set **Action** → Launch Application  
5. Select **Square Home**  
6. Save and test  

Now the phone restarts directly into the dashboard instead of a standard Android screen.

---

## Current Status

At this point, the phone is close to how I originally pictured it. The structure is in place, but I still need to:

- Connect each tile to its final function  
- Add some visual polish  
- Finish automation (like Shabbat reminders)

This is still a work in progress, and I’ll update this guide as I continue improving the setup.