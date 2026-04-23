# How to Root an Android Phone

Like many people, I have a drawer full of old devices that are no longer worth selling but still feel too useful to throw away. I had been looking for ways to repurpose some of that unused hardware, and one weekend I finally decided to start experimenting.

My first test project was an older Xiaomi phone. I had several ideas for it, but before turning it into something new, I wanted to clean it up, reclaim storage space, and gain more control over how it behaved.

That meant rooting it.

Rooting gives you administrative access to Android and allows deeper control over the operating system. With root access, you can remove unnecessary apps, reduce background activity, automate startup behavior, and customize the device in ways normally restricted by the manufacturer.

This was my first time rooting a phone. Below is the process I followed, including how rooting works, how to check whether a device is compatible, the risks involved, and how to complete the setup safely.

---

## What Rooting Means

Android is built on Linux. By default, phone manufacturers restrict access to core system areas. Rooting removes those restrictions and grants elevated permissions, commonly referred to as **root access**.

With root access, you can:

- Remove preinstalled system apps  
- Change startup behavior  
- Run advanced automation tools  
- Improve performance on older hardware  
- Install specialized utilities  
- Repurpose devices for dedicated tasks  

In my case, rooting let me strip the phone down to only the apps I needed for the project and noticeably improved battery life.

---

## Risks of Rooting Your Phone

For my first attempt, I intentionally used a spare device rather than a phone I depended on every day. Rooting can be useful, but it comes with real trade-offs.

- **Security risks:** Apps granted root access can make deep system changes. That is helpful when you trust the app, but risky if you install something questionable.  
- **Stability problems:** Android components often depend on each other. Removing or changing the wrong package can cause crashes, battery drain, broken features, or boot loops.  
- **Update issues:** Rooted phones do not always handle normal software updates cleanly. Updates may fail, remove root access, or require manual reinstallation.  
- **App compatibility:** Some banking, payment, streaming, or workplace security apps may refuse to run on rooted devices.  
- **Data loss:** Unlocking the bootloader often wipes the phone completely. Back up your data first.  
- **More maintenance:** A rooted phone usually needs more hands-on management than a stock device.  

---

## Before You Begin

Complete the following preparation steps before making any system changes.

1. **Check device compatibility**  
   Search online for your exact phone model and confirm that the bootloader can be unlocked. Devices from Google Pixel, Xiaomi, and OnePlus are generally easier to root than carrier-locked devices.

2. **Back up your data**  
   Save photos, contacts, files, notes, and anything else you want to keep.

3. **Download the required tools**

   - Android Platform Tools (ADB and Fastboot)  
   - Magisk (latest release)  
   - Device USB drivers (if required)  
   - Stock firmware for your exact device model  

4. **Enable Developer Options**

   On the phone:

   - Open **Settings > About Phone**  
   - Tap **Build Number** seven times  
   - Open **Developer Options**  
   - Enable **USB Debugging**  
   - Enable **OEM Unlocking**  

---

## Root the Device

The exact process varies by manufacturer, but most modern Android devices follow the same general workflow.

---

## Phase 1: Prepare the Connection

### Step 1: Verify ADB Access

Connect the phone to your computer with a USB cable.

Run:

```bash
adb devices
```

Approve the USB debugging prompt on the phone.

Your device should appear with a status of `device`.

---

### Step 2: Reboot to Bootloader

Run:

```bash
adb reboot bootloader
```

The phone should restart into Fastboot or bootloader mode.

---

## Phase 2: Unlock the Bootloader

### Step 3: Unlock the Device

Run one of the following commands:

```bash
fastboot flashing unlock
```

or

```bash
fastboot oem unlock
```

Use the volume and power buttons on the phone to confirm.

!!! warning
    Unlocking the bootloader usually performs a factory reset.

Once the phone reboots, repeat the Developer Options steps to re-enable USB Debugging if needed.

---

## Phase 3: Patch the Boot Image

### Step 4: Obtain the Correct Firmware

Download the stock firmware that exactly matches the Android version currently installed on the phone.

Extract the package and locate:

```text
boot.img
```

!!! warning
    Using a boot image from the wrong firmware version can prevent the phone from booting.

---

### Step 5: Patch the Boot Image with Magisk

1. Install the Magisk APK on the phone.  
2. Copy `boot.img` to the device.  
3. Open **Magisk**.  
4. Tap **Install**.  
5. Select **Patch a File**.  
6. Choose `boot.img`.  

Magisk creates a patched image file, usually named something similar to:

```text
magisk_patched.img
```

Copy the patched image back to your computer.

---

## Phase 4: Flash the Patched Image

### Step 6: Flash Root Access

Reboot into Fastboot mode if needed:

```bash
adb reboot bootloader
```

Then flash the patched image:

```bash
fastboot flash boot magisk_patched.img
```

Reboot:

```bash
fastboot reboot
```

The first boot may take longer than normal.

---

## Phase 5: Verify Root

### Step 7: Confirm Success

Once Android loads:

1. Open **Magisk**  
2. Confirm that Magisk shows as installed  
3. Grant root access only to apps you trust  

You can also install a root checker app for independent confirmation.

---

## Next Steps

Once rooted, you can begin tailoring the device to your needs.

### Remove Bloatware

Uninstall or disable system apps you do not need.

### Improve Performance

Reduce unnecessary background processes and startup tasks.

### Build a Dedicated Device

Use the phone as:

- A wall dashboard  
- Smart home panel  
- Music player  
- Kitchen organizer  
- Family calendar  
- Media controller  

### Manage Root Permissions

Review root access regularly inside Magisk and revoke anything unnecessary.

---

## Troubleshooting

### Device Bootloops After Flashing

The patched boot image likely did not match the installed firmware version.

Flash the original boot image:

```bash
fastboot flash boot boot.img
fastboot reboot
```

Then repeat the patching process with the correct firmware version.

---

### ADB Shows `unauthorized`

Unlock the phone screen and approve the USB debugging prompt.

If needed:

- Disable USB Debugging  
- Re-enable it  
- Reconnect the cable  

---

### OEM Unlocking Is Greyed Out

Some manufacturers or carriers restrict bootloader unlocking. Check whether the device requires an official unlock request process.

---

### Rooted Apps Are Detected

Enable Magisk features such as Zygisk and configure the deny list for affected apps.

---

## Final Thoughts

Rooting is not something to rush, but it can be a practical way to extend the life of older hardware.

For this project, it was the first step in turning an unused phone into something genuinely useful instead of leaving it in a drawer. With some care and patience, a retired device can become a focused tool again.
