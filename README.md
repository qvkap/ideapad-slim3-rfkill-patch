# Linux Kernel Quirk for Lenovo IdeaPad Slim 3 16IRH10

## Issue
On newer IdeaPad models such as the **Lenovo IdeaPad Slim 3 16IRH10** (machine type `83K2`), the EC/ACPI firmware reports erratic rfkill radio states through `VPCCMD_R_WIFI` and `VPCCMD_R_RF`. Because these laptops lack a physical hardware rfkill switch, the `ideapad-laptop` platform driver inadvertently registers soft and hard rfkill blocks against the native Wi-Fi (`iwlwifi`) and Bluetooth interfaces without any user intervention.

## Solution
This patch introduces a `no_rfkill` module parameter and adds the `Lenovo IdeaPad Slim 3 16IRH10` to a `no_rfkill_list` DMI quirk table. This bypasses the defective ACPI rfkill registration and lets native hardware drivers manage radio power states cleanly.

- **Target driver:** `drivers/platform/x86/lenovo/ideapad-laptop.c`
- **Checkpatch status:** `0 errors, 0 warnings, 64 lines checked`
- **Subsystem:** `platform-driver-x86`

## Patch File
The standard git-formatted patch is available at [`0001-platform-x86-ideapad-laptop-add-no_rfkill-quirk-for-.patch`](./0001-platform-x86-ideapad-laptop-add-no_rfkill-quirk-for-.patch).

## Applying
```bash
cd /path/to/linux-source
git apply 0001-platform-x86-ideapad-laptop-add-no_rfkill-quirk-for-.patch
```
