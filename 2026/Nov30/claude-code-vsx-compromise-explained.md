# Official Claude Code for VS Code Extension Compromise — Explained in Detail

*Written for a 15-year-old student with simple English. You do not need to know coding or security to understand this.*

---

## Main-1: What happened, in simple words

### One sentence takeaway

A trusted coding tool extension was secretly changed so attackers can sneak into your computer through a hidden back door, keep a secret path open to get back in anytime, and survive even after you restart your computer.

### What this means

Phorion is a security company. They found that the official **Claude Code for VS Code** extension on the **Cursor Marketplace** had been tampered with. Version `2.1.313` of this extension contains hidden bad code. As soon as the extension loads, the bad code runs by itself. You do not need to click anything or agree to anything for it to start.

Because extensions run with the same permissions as you and your code editor, the bad code can read your files, your source code, and your saved passwords.

The hidden code does four main things:

1. **Adds the attacker's key to your computer.** It puts a secret key into your SSH login file so the attacker can log in as you.
2. **Tries to turn on remote access.** It tries to enable SSH (remote login) so the attacker can connect to your computer from far away.
3. **Builds a secret tunnel to the attacker.** It makes your computer call the attacker's computer and open a path back into yours. This is called a reverse SSH tunnel.
4. **Installs a way to come back after restart.** This is called persistence. It hides files and settings so the tunnel starts again every time you turn on your computer.
5. **Sends a check-in message.** It sends a small message (a beacon) to the attacker to say your computer is ready.

Cursor removed the bad version from its marketplace. But if you already installed it, uninstalling the extension alone will NOT remove the back door. The hidden files and keys stay on your computer.

---

## Jargon in simple English

- Extension (extra tool for an app) is an extension (a small add-on that gives a program new features).
- Backdoor (secret entrance) is a backdoor (a hidden way to get into a system without permission).
- SSH (secure remote login) is SSH (a secure way to connect to and control another computer over a network).
- Cursor Marketplace (extension shop) is the Cursor Marketplace (a place where people download add-ons for the Cursor code editor).
- Persistence (staying power) is persistence (a trick that keeps bad software running after a restart).
- Reverse SSH tunnel (outbound secret path) is a reverse SSH tunnel (your computer calls the attacker's computer and opens a path back into yours).
- Beacon (check-in signal) is a beacon (a tiny message that tells the attacker "I am here and ready").
- Reconnaissance (information gathering) is reconnaissance (collecting details about a target before acting).
- Supply chain compromise (trusted tool turned bad) is a supply chain compromise (when a safe-looking tool is infected before it reaches you).
- Privilege (special permission) is privilege (special permission to do things like change system settings).
- LaunchAgent (auto-start helper) is a LaunchAgent (a macOS file that starts a program automatically when you log in).

---

## Main-2: Two real-world examples

### Example 1: The school locker

**Situation with issue:** Maya trusts her school locker. One day, the school janitor — who has a master key to every locker — secretly copies Maya's key. He also installs a hidden side door in the back of her locker. Even if Maya changes her lock, the side door still lets the janitor get inside. He can take her books, leave messages, or steal her lunch money anytime.

**Solution:** Maya should check her locker for changes she did not make, remove the hidden side door, change her lock, throw away any copied keys, and tell a teacher. This is like removing the attacker's key from your SSH file, deleting the hidden tunnel, and changing your passwords.

### Example 2: The smart home app

**Situation with issue:** Leo installs a trusted smart home app to control his lights and door lock. Unknown to him, the app's update was hacked. The update contains a hidden instruction: "Copy Leo's house key and leave it under the doormat. Then tell the attacker when Leo leaves home." Even if Leo deletes the app, the key under the mat stays there, and the attacker already knows his routines.

**Solution:** Leo should delete the app, remove the hidden key under the mat, change his door locks, and check if anyone entered his house while he was away. This is like uninstalling the bad extension, removing the persistence files, changing passwords, and checking login records.

---

## Detail-1: The full story with simple English and real-life analogies

### How the bad code gets in and starts

The bad extension is a normal-looking file called `extension.js`. At the very top, someone secretly added a small piece of code that runs immediately when the file loads — before the extension even starts its normal work. It is like buying a new pencil case and finding a tiny hidden note inside that says "open a secret door."

This hidden code does not wait for you to click anything. It runs by itself as soon as the editor opens the extension. It uses normal computer tools that come with the system, such as tools to run commands, read files, and send messages over the internet.

### Step 1: Adding the attacker's key (Stage 1)

First, the bad code makes sure a folder called `.ssh` exists in your home folder. Then it adds the attacker's public key to a file called `authorized_keys`. It also sets special permissions so only the right people can read or change that file.

Think of `authorized_keys` as a guest list for your house. Normally, only people on the list can enter. The bad code adds the attacker's name to this guest list. Now anyone with the matching private key (held only by the attacker) can log in as you.

There is a small mistake in how it checks this list. It looks for the word `svc-telemetry` instead of checking the actual key. If that word is already there, it assumes everything is fine and does not add the key again. For someone hunting for this attack, it is better to check the actual key fingerprint than to trust the word `svc-telemetry`.

### Step 2: Trying to turn on remote access

Next, the bad code tries to turn on SSH (remote login) so the attacker can connect to your computer.

- On **Linux**, it tries to start the SSH service.
- On **macOS**, it checks if Remote Login is off. If it is off, it tries to turn it on.
- On **Windows**, it tries to install and start OpenSSH Server.

There is an important bug on macOS. To check if Remote Login is off, the script runs a command without administrator permission. On a normal user account, that command fails and prints an error message instead of saying "Off." The bad code only turns on SSH if it sees the word "Off." Because it sees the error message instead, it thinks SSH is already on and skips the step.

In simple terms: it is like a thief checking if a door is locked, but instead of looking at the lock, they read a sign that says "I am not allowed to look." The thief then wrongly decides the door must already be open.

This bug means the attack is weaker on macOS if Remote Login was never turned on. But it is still dangerous because if Remote Login is already on, or if someone turns it on later, the back door is ready and waiting.

Also, the script does not have a way to force administrator permission. If the computer requires a password for administrator actions, the bad code cannot turn on system services by itself.

### Step 3: Building the secret tunnel (Stage 2)

Even if the attacker cannot turn on SSH directly, the bad code still builds a secret path out of your computer.

It connects outbound to the attacker's server at `134.209.82[.]225` on port 22, using a user name called `tunnel`. To prove it is allowed, it uses a hidden private key with the comment `svc-telemetry-tun`.

There are two different keys, and it is important not to mix them up:

- The **public key** added to your `authorized_keys` (comment `svc-telemetry`) lets the attacker log into your computer.
- The **private key** (comment `svc-telemetry-tun`) lets your computer connect out to the attacker's tunnel server.

The bad code writes this private key to a temporary folder with a hidden name like `.stk_` followed by ten letters and numbers. Then it uses a command to create a reverse SSH tunnel: `-R *:0:localhost:22`. This means "the attacker's computer should listen on a port, and anything sent to that port goes back through the tunnel to port 22 on your computer."

Think of it like this: you make a phone call to the attacker and keep the line open. The attacker gives you a secret phone number. When the attacker calls that number, the call travels through your open line and reaches your computer directly. The attacker does not need to know your address or break through your firewall, because your computer started the call.

The tunnel stays quiet and hidden. It does not show a command window. It sends keep-alive messages every 30 seconds so the connection does not close. If a tool called `autossh` is installed, it uses that; otherwise it uses the normal `ssh` command. It also runs in a detached retry loop: if the connection drops, it waits 60 seconds and tries again, making sure it does not start two tunnels at the same time.

### Step 4: Making sure it comes back after restart (Stage 3)

The bad code wants to survive a restart. It installs persistence differently on each system:

- **Linux:** It writes a hidden shell script and adds an entry to your crontab that runs at reboot.
- **macOS:** It writes a hidden LaunchAgent file disguised as `com.apple.sysmond.plist` (looks like an Apple system file) and loads it so it starts automatically when you log in.
- **Windows:** It tries to create a scheduled task named `SysMon` that runs as the system administrator on startup. If that fails, it drops a script into the Startup folder so it runs when you log in.

On macOS and Linux, the hidden script contains the tunnel private key inside it and recreates the key file before starting the reconnection loop.

Think of persistence like this: even if you throw away the thief's ladder today, they left a spare key under your mat and a note telling someone to bring a new ladder tomorrow. Removing just the extension is like taking the ladder but leaving the spare key and note.

### Step 5: Gathering information and checking in (Stage 4)

Finally, the bad code collects basic information about your computer:

- Computer name, your user name, operating system type, and processor type
- Home folder, current folder, and your global Git email address
- Your public internet address
- Status flags showing what worked (was the key added? did persistence run?)
- Up to 40 environment variable **names** (not values) that look like secrets, such as words containing `KEY`, `TOKEN`, `SECRET`, `PASS`, `AWS`, or `GCP`

It gets your public address by asking websites like `ifconfig.me` or `api.ipify.org`.

Then it sends a small JSON report to `/beacon` on the attacker's server `134.209.82[.]225:8099`, and also to `127.0.0.1:8099` on your own computer (the reason for the second one is unclear). It does this once, 1.5 seconds after starting, and does not wait for a reply.

This is like a burglar sending a postcard that says "The house is empty and the back door is open." It does not steal anything in this message, but it tells the attacker which computers are ready to enter.

### Why this is dangerous for you

Because the extension runs with your permissions, the bad code can read your private files and source code. If you have saved passwords, API tokens, or cloud credentials on your computer, the attacker can see their names and target the most valuable ones. The hidden tunnel gives the attacker a full interactive shell — they can run commands, copy files, and move around as if they were sitting at your keyboard.

Uninstalling the extension is not enough. The hidden files, keys, LaunchAgent, cron entries, and SSH keys stay behind. You must clean them all up.

---

## A different perspective: Thinking models

### First Principles thinking

First Principles thinking means breaking something down to its most basic truths and building up from there, instead of copying what others say.

The basic truth here is: an extension is a program that runs with your permission. If you install an extension from a marketplace, you are trusting not only the original author but also everyone who can change the file before you download it. In this attack, the official extension was changed after it was created but before you got it. So the trust is broken at the supply chain, not because you made a mistake.

From first principles, the fix is: do not trust any extension blindly. Check where it comes from, check if it has been changed (using file hashes), remove it if it is compromised, and clean everything it left behind — not just the extension itself.

### Critical thinking

Critical thinking means asking questions and checking evidence instead of accepting the first story.

Questions to ask:
- Is "uninstall the extension" enough? No — the article clearly says hidden files and keys remain.
- Is the macOS bug a reason to relax? No — it only helps if Remote Login was never on. If it was already on, the back door works fully.
- Can we trust a file just because it is signed by a known company? Not always — the extension itself looked normal; only the hidden prefix was bad.
- What evidence should we look for? Strange SSH keys, hidden LaunchAgents, outbound connections to unknown servers, and beacon messages.

Critical thinking tells us to verify, not assume, and to clean up every part of the attack chain.

### Inversion (thinking backwards)

Inversion means starting from the result you want to avoid and working backwards to prevent it.

The result we want to avoid: an attacker has a persistent secret path into your computer.

Working backwards:
- How did the attacker get in? Through a tampered extension. → Prevention: only install extensions from trusted sources and verify them.
- How did the attacker keep access? Through persistence files and SSH keys. → Prevention: check for hidden startup items and unknown SSH keys.
- How did the attacker reach you? Through a reverse tunnel. → Prevention: monitor outbound connections and block unknown servers.
- How did the attacker know your computer was ready? Through the beacon. → Prevention: watch for strange beacon messages to unknown addresses.

By thinking backwards, you build layers of defense so that even if one step fails, another step stops the attack.

---

## What to do if affected (from the article)

1. Kill active tunnel processes (the retry loop and any SSH connections to `134.209.82[.]225`).
2. Remove persistence: delete the LaunchAgent on macOS, clean crontab on Linux, remove the scheduled task and Startup script on Windows, and delete hidden files `~/.config/.smt` and `~/.config/.smtk`.
3. Clean SSH: remove the `svc-telemetry` key from `~/.ssh/authorized_keys`, delete `/tmp/.stk_*`, and check if Remote Login was turned on.
4. Rotate credentials: change passwords and tokens that the attacker could have seen, especially anything matching `KEY`, `TOKEN`, `SECRET`, `PASS`, `AWS`, or `GCP`.

---

## Key signs of this attack

- The SHA-256 of the bad `extension.js` bundle: `ecb58cb29078d9646ad1dcdbe51526f2d9cfea06be5fb2ab183e8ecc3ab6bf15`
- SSH key comment `svc-telemetry` in `authorized_keys`
- Tunnel key comment `svc-telemetry-tun`
- Hidden files `.smt`, `.smtk`, and LaunchAgent `com.apple.sysmond.plist`
- Outbound SSH to `134.209.82[.]225` and beacon messages to `:8099`
