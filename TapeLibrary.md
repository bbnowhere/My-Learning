### Hardware Specifications & Device Paths (Updated)

**Host Server:** ``
**Storage Controller:** IBM ServeRAID M5210

**The Tape Library (The Robotic Arm / Medium Changer)**

* **Make & Model:** **FUJITSU ETERNUS LT S2**
* **SCSI Path:** `/dev/sg6` (Also mapped as `/dev/sch0`)
* **Total Drives:** 1
* **Total Tape Slots:** 8
* *Note: This is the chassis that handles the mechanical movement of the tapes.*

**The Tape Drive (The Reader/Writer)**

* **Make & Model:** **IBM ULTRIUM-HH6** (Half-Height LTO-6)
* **SCSI Path:** `/dev/sg5` (This is the control path, but data is written to `/dev/nst0`)
* **Data Paths:** `/dev/nst0` (Non-rewinding) / `/dev/st0` (Rewinding)
* **Drive Assignment:** Drive 0
* *Note: This is the actual magnetic drive that reads/writes the LTO-6 tapes.*

### 1. Checking System & Device Status

**Check the tape library (robotic arm & slots) status:**
See which slots are empty, which are full, and if a tape is currently sitting in the drive (Data Transfer Element 0).

```bash
mtx -f /dev/sg6 status

```

*Or using your script:* `/root/automate/tape_manager.py status`

**Check the tape drive status:**
See if the drive is actively spinning, what block it is on, or if a tape is loaded (`ONLINE`).

```bash
mt -f /dev/nst0 status

```

**Check if any process is actively using the tape drive:**
If this returns nothing, the drive is idle. If it returns a list, a backup or script is currently accessing the drive.

```bash
lsof /dev/nst0

```

**Search for running backup background jobs:**
Check if your `cron` jobs or manual backup scripts are actively running in the background.

```bash
ps -ef | grep -E "backup|tar|mtx" | grep -v grep

```

---

### 2. Moving Tapes (Robotic Arm)

*Note: The drive must not be in use before moving tapes.*

**Unload the tape from the drive back to Slot 1:**

```bash
mtx -f /dev/sg6 unload

```

*Or using your script:* `/root/automate/tape_manager.py unload 1`

**Load a tape from a specific slot (e.g., Slot 1) into the drive:**

```bash
mtx -f /dev/sg6 load 1 0

```

*Or using your script:* `/root/automate/tape_manager.py load 1`

**Refresh the library's internal inventory:**
Forces the robotic arm to scan the chassis and update its memory of where the tapes are.

```bash
mtx -f /dev/sg6 inventory

```

*Or using your script:* `/root/automate/tape_manager.py inventory`

---

### 3. Tape Drive Control (Magnetic Operations)

*Note: These commands speak directly to the drive mechanism, bypassing the robotic arm.*

**Eject/Offline the tape:**
Forces the drive to rewind the tape and release its mechanical grip so the robotic arm can grab it.

```bash
mt -f /dev/nst0 offline

```

*Or using your script:* `/root/automate/tape_manager.py offline`

**Rewind the tape to the beginning:**

```bash
mt -f /dev/nst0 rewind

```

*Or using your script:* `/root/automate/tape_manager.py rewind`

**Erase the tape (DESTRUCTIVE):**
Wipes all data currently on the loaded tape.

```bash
mt -f /dev/nst0 erase

```

*Or using your script:* `/root/automate/tape_manager.py erase`

---

### 4. Running & Monitoring Backups

**Manually start the weekly backup script:**

```bash
cd /root/automate/
./run_weekly_backup.sh

```

**Watch the master backup log in real-time:**
Because your script names the log file based on the date, you can use this command to instantly watch today's log file as it is being written. *(Press `Ctrl+C` to exit).*

```bash
tail -f /data2/tape/weekly_$(date +%Y_%m_%d).log

```
