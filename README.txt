ROB RUN - ESP + aimbot package
==============================

WHAT THE TARGET MACHINE NEEDS
-----------------------------
- Windows 10 or 11, 64-bit.
- An account with administrator rights (not a standard account).
- Memory Integrity / Core Isolation / HVCI - OFF.
  Windows Security -> Device security -> Core isolation details -> Memory integrity: Off.
  Without this the driver does not load. It needs a restart after you turn it off.
- WebView2 Runtime installed (Windows 11 normally ships it already).
  If not: https://developer.microsoft.com/microsoft-edge/webview2/
- ROB RUN installed and working on your account.
- Antivirus can quarantine the files. Add the folder to the exclusion list
  BEFORE you unzip, or kd2.exe or driver.sys will be gone when you look.

EXACT ORDER - do not skip steps
-------------------------------
1. Restart the machine (clean boot, no Fast Startup if you can help it).
2. Unzip the WHOLE folder into a simple path, e.g. C:\ROBRUN
   Do not run from inside the archive. Do not put it in OneDrive or on a
   synced Desktop.
3. Double-click MAP-DRIVER.bat -> Yes at the UAC prompt.
   At the end you must see:  kd2.exe exit code: 0
   Anything else: restart, then run it once more.
   Once per boot. Not twice.
4. Start ROB RUN and get into a match. Not the menu.
5. Double-click START.bat -> Yes at the UAC prompt.
6. THE FIRST TIME TAKES 60-100 SECONDS. The overlay scans game memory to find
   the addresses. It shows "Waiting for game".
   Do NOT press Ctrl+End. Do NOT close anything. Wait.
   After the first scan, later launches are instant as long as the game has not
   been restarted.
7. When the overlay appears, it is ready.

KEYS
----
F1              = menu (ESP, aim, colours, radar)
Insert          = show / hide the overlay
End             = close everything while the menu is open (writes CoreRR.cfg)
Ctrl+End        = close everything, at any time
Right click     = aimbot (hold)

F1 and Insert are bare: pressing F1 still reaches the game untouched, nothing
here swallows it. End is bare only while the menu is open, because the game
binds End to drop-crouch and unloading is destructive - mid-fight that key
stays the game's. ReShade toggles its effects on bare End, Steam uses Shift+Tab
and the Discord overlay uses Shift+`, so the closed-menu behaviour is
deliberate. The Ctrl+F1 / Ctrl+Insert registrations stay active as
alternatives, and RR_HK_MOD=0 makes bare End work everywhere, e.g.
  set RR_HK_MOD=0 && RobRunPort.exe

RESHADE, STEAM AND DISCORD OVERLAYS
-----------------------------------
They run inside the game process and hook its swap chain. This overlay is a
separate topmost window drawn by the desktop compositor, so the two do not
share a frame and cannot break each other's rendering. Three things still
matter:

- Keep the ReShade overlay CLOSED while you play. With it open, ReShade takes
  the mouse, so the game stops receiving look input and the aimbot moves the
  ReShade cursor instead of the camera.
- Use borderless / windowed fullscreen, not exclusive fullscreen. Exclusive
  fullscreen bypasses the compositor and a topmost window cannot be drawn over
  it - the ESP disappears and aim drifts.
- ReShade's own keys stay ReShade's: Home opens it, End toggles its effects
  while this overlay's menu is closed. Open the menu with F1 and End becomes
  the unload key instead.

IF IT DOES NOT WORK
-------------------
- "Waiting for game" for more than 2 minutes: the game is not in a match, or
  Ctrl+End was pressed too early on the first run. Restart START.bat and wait.
- MAP-DRIVER returned an exit code other than 0: Memory Integrity is probably
  still on. Turn it off, restart, run it once more.
- The overlay starts but sees nothing: the game received an update and the
  addresses no longer match. It has to be rebuilt with new offsets.
- Aim fires but does not stick: in the menu raise aim cal 500 -> 800 -> 1200.
- Aim does not move the camera at all: switch the game to borderless / windowed.
- Menu or exit key does nothing: press Ctrl as well. If you set RR_HK_MOD=0,
  remove it - then the bare keys are active again.

RULES
-----
- Edit CoreRR.cfg only with the overlay CLOSED. On exit the overlay rewrites the
  file from its own memory and wipes your changes.
- The driver stays loaded until restart. The registry key is volatile and is
  gone after a reboot. Hence: reboot -> MAP-DRIVER once -> game -> overlay.
- Do not move the folder while it is running.
- START-MASQ.bat is optional: it copies the overlay into %SystemRoot%\Temp as
  dllhost.exe. Same keys.
