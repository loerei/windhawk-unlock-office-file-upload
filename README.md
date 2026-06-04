# Unlock Office Files for Upload

A [Windhawk](https://windhawk.net/) mod that removes the **"This file is in use"** error when you try to upload or attach an Office file while it is open in Word, Excel, or PowerPoint.

## The Problem

When a `.docx`, `.xlsx`, `.pptx` (or any legacy Office format) is open in Word/Excel/PowerPoint, Windows places a sharing lock on the file. Any application that then tries to open that file for reading — a browser file picker, Outlook's attach dialog, Teams, Discord, etc. — gets an `ERROR_SHARING_VIOLATION` from the OS and shows you:

> **This file is in use. Enter a new name or close the file that's open in another program.**

## How It Works

The mod hooks `CreateFileW` in every process (injected by Windhawk). When a read-only open attempt on a tracked Office file fails with a sharing violation, it:

1. Makes a temporary copy of the file in `%TEMP%` (`~ul_XXXXXXXX_filename.docx`)
2. Redirects the open call to the copy — the uploader gets a valid handle with no error
3. Hooks `CloseHandle` to delete the temp copy automatically once the uploader is done

The entire operation is transparent — no popups, no need to close Word first.

## Limitation

The temp copy reflects the last **saved** state of the file. Unsaved in-memory edits in Word are not included. This is inherent to how Windows file locking works — the on-disk file is what can be read.

## Supported Extensions

Default list (configurable in Windhawk settings):

| Format | Extensions |
|---|---|
| Word | `.docx` `.docm` `.doc` |
| Excel | `.xlsx` `.xlsm` `.xls` |
| PowerPoint | `.pptx` `.pptm` `.ppt` |

## Installation

### Via Windhawk Catalog
Search for **"Unlock Office Files for Upload"** in the Windhawk app.

### Manual
1. Open Windhawk → **New Mod**
2. Paste the contents of [`mod.wh.cpp`](mod.wh.cpp)
3. Click **Compile** (Ctrl+B)
4. Toggle the mod **on**

## License

MIT
