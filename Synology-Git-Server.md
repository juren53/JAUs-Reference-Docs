# Synology Git Server — Setup and Usage

**Date:** 2026-10-29 1649 CDT

Private git hosting on JAU's Synology NAS, reached over SSH with key-based login, plus a **Gitea** web interface.

---

## At a glance

| Item | Value |
|---|---|
| NAS address | `192.168.1.105` |
| SSH port | **222** (not the default 22) |
| User | `juren` (member of the `administrators` group) |
| Repository folder | `/volume1/git` (DSM shared folder `git`) |
| Git on NAS | 2.39.1 (Synology **Git Server** package) |
| Login method | SSH key (`~/.ssh/id_ed25519`), no password |
| Repo URL form | `nas:/volume1/git/<name>.git` |
| Test repo | `/volume1/git/test.git` (safe to delete) |
| Web interface | **Gitea** at http://192.168.1.105:3000 (see [Gitea web interface](#gitea-web-interface)) |
| Gitea SSH | port **2222**, alias `gitea`, URL form `gitea:juren/<name>.git` |

---

## What was done

### In DSM (http://192.168.1.105:5000)

1. **Git Server** package installed. In the app, `juren` has **Allow access** checked.
2. **SSH enabled** in Control Panel → Terminal & SNMP → Terminal, on port **222**.
3. **User home service enabled** in Control Panel → User & Group → Advanced. Your SSH key lives in `/var/services/homes/juren/.ssh/`.
4. **`juren` added to `administrators`**. DSM only allows SSH logins for members of this group. The delegated-admin roles that let `juren` manage packages and services don't count.
5. **Shared folder `git`** created on Volume 1, with Read/Write for `juren`.

### On the Windows PC

1. Created `C:\Users\juren\.ssh\config` with a `nas` shortcut:

   ```
   Host nas
       HostName 192.168.1.105
       Port 222
       User juren
       IdentityFile ~/.ssh/id_ed25519
   ```

2. Installed the existing public key on the NAS. Run this once from a regular PowerShell window, which asks for the DSM password:

   ```powershell
   type .ssh\id_ed25519.pub | ssh nas "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 755 ~ && chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys"
   ```

3. Tested a full cycle: created a bare repo, cloned it, committed, pushed, then cloned again from scratch and confirmed the commit.

---

## Everyday use

The commands below work from any machine that has the `nas` entry in its SSH config. That includes Windows (Git Bash or PowerShell) and Linux.

### Create a new repository on the NAS

```bash
ssh nas git init --bare -b main /volume1/git/<name>.git
```

Repositories on the NAS are always **bare**: they hold history but no working files. You never edit files there directly.

### Clone it

```bash
git clone nas:/volume1/git/<name>.git
```

### Put an existing local project on the NAS

```bash
ssh nas git init --bare -b main /volume1/git/<name>.git
cd <project>
git remote add nas nas:/volume1/git/<name>.git
git push -u nas main        # use 'master' if that is the local branch name
```

### Push to GitHub and the NAS together

This makes `git push` send to both places:

```bash
git remote set-url --add --push origin git@github.com:juren53/<name>.git
git remote set-url --add --push origin nas:/volume1/git/<name>.git
git remote -v               # origin should show two (push) lines
```

`git pull` still fetches from GitHub. Pull from the NAS explicitly with `git pull nas main`.

### List, rename, or delete repositories

```bash
ssh nas ls /volume1/git
ssh nas mv /volume1/git/old.git /volume1/git/new.git
ssh nas rm -rf /volume1/git/test.git      # permanent; DSM #recycle may not catch it
```

### Branch name tip

New repos on the Windows PC default to `master`, but the NAS and GitHub repos use `main`. To make `main` the default everywhere, run this on each workstation:

```bash
git config --global init.defaultBranch main
```

---

## Setting up a Linux LMDE workstation

Run these steps on the LMDE machine.

### 1. Install git and the SSH client

```bash
sudo apt update
sudo apt install git openssh-client
git config --global user.name  "Jim U'Ren"
git config --global user.email "jim.uren@gmail.com"
git config --global init.defaultBranch main
```

### 2. Create an SSH key (skip if `~/.ssh/id_ed25519` already exists)

```bash
ssh-keygen -t ed25519 -C "juren@lmde"
```

Press Enter to accept the default location. A passphrase is optional.

### 3. Add the `nas` shortcut

```bash
mkdir -p ~/.ssh && chmod 700 ~/.ssh
cat >> ~/.ssh/config <<'EOF'
Host nas
    HostName 192.168.1.105
    Port 222
    User juren
    IdentityFile ~/.ssh/id_ed25519
EOF
chmod 600 ~/.ssh/config
```

### 4. Copy the key to the NAS (asks for the DSM password once)

```bash
ssh-copy-id nas
```

`ssh-copy-id` reads the port and user from the `nas` entry. It appends to `authorized_keys`, so keys from other machines, including the Windows PC, are kept.

If key login fails afterward, fix the home-folder permissions on the NAS. Synology ignores keys when the home folder is group- or world-writable:

```bash
ssh nas "chmod 755 ~ && chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys"
```

### 5. Test

```bash
ssh -o BatchMode=yes nas 'echo KEY_LOGIN_OK; git --version'
git clone nas:/volume1/git/test.git /tmp/nas-test && git -C /tmp/nas-test log --oneline
```

`KEY_LOGIN_OK` followed by a commit list means the workstation is ready.

### Status: done (2026-10-29)

The LMDE workstation is set up and tested:

- `~/.ssh/config` has `nas` (port 222) and `gitea` (port 2222) entries, both using `~/.ssh/id_ed25519`. The earlier config was backed up to `~/.ssh/config.bak-20261029`.
- The key was copied to the NAS with `ssh-copy-id nas`. It was also added in Gitea under the SSH key name **Gitea**.
- Gitea's port-2222 host key was added to `known_hosts`.
- `init.defaultBranch` is `main`.
- Tests passed: key login (`KEY_LOGIN_OK`), cloning and pushing to `test.git`, `ssh gitea` ("successfully authenticated"), and cloning `gitea:juren/hstl-hours.git`.

An older `synology` alias in `~/.ssh/config` (port 222, `~/.ssh/id_rsa`) didn't work, because that key isn't in the NAS's `authorized_keys`. It was removed on 2026-10-29. Use `nas` instead. For scp, add `-O`, which DSM needs (`scp -O <file> nas:<path>`).

---

## Gitea web interface

Synology's Git Server package has no web page. **Gitea** provides a GitHub-style site for browsing code, commits and diffs, and for issues and pull requests.

### At a glance

| Item | Value |
|---|---|
| Web address | http://192.168.1.105:3000 (login required; public sign-up disabled) |
| Admin account | `juren` |
| Version | Gitea 28.2.0, `linux-arm64` binary (the DS218 is ARM; Container Manager/Docker isn't available on it) |
| Install folder | `/volume1/git/gitea` |
| Config | `/volume1/git/gitea/custom/conf/app.ini` (mode 600; contains secrets) |
| Database | SQLite, `/volume1/git/gitea/data/gitea.db` |
| Gitea's repositories | `/volume1/git/gitea/repositories/<owner>/<name>.git` |
| Logs | `/volume1/git/gitea/log/` (`gitea.log`, `stdout.log`) |
| Runs as | `juren` (not root) |
| SSH | Gitea's built-in SSH server on port **2222** (DSM's own SSH stays on 222) |
| Start script | `/volume1/git/gitea/start.sh` |

### How it was installed

All steps were run over SSH as `juren`:

1. Downloaded `gitea-28.2.0-linux-arm64` and its `.sha256` from the Gitea GitHub release, verified the checksum, and linked it as `/volume1/git/gitea/gitea`.
2. Wrote `app.ini` with these settings:
   - SQLite database, `HTTP_PORT = 3000`, `ROOT_URL = http://192.168.1.105:3000/`
   - `START_SSH_SERVER = true` on port 2222
   - `DEFAULT_BRANCH = main`, `INSTALL_LOCK = true`
   - `DISABLE_REGISTRATION = true`, `REQUIRE_SIGNIN_VIEW = true`, `OFFLINE_MODE = true`
   - Secrets generated with `gitea generate secret`
3. Ran `gitea migrate` to create the database (this takes a few minutes on the DS218), then created the admin user. The admin user was created with a temporary password that must be changed at first login.
4. Started Gitea with `start.sh`. The script does nothing if Gitea is already running.

### Start Gitea automatically at boot (DSM Task Scheduler)

Control Panel → **Task Scheduler** → Create → **Triggered Task** → **User-defined script**:

- **General:** Task name `Gitea`, User **juren**, Event **Boot-up**, Enabled
- **Task Settings → Run command:** `/volume1/git/gitea/start.sh`

To start or check it by hand:

```bash
ssh nas /volume1/git/gitea/start.sh
ssh nas 'ps -ef | grep "[g]itea web"'
ssh nas 'tail -20 /volume1/git/gitea/log/gitea.log'
```

To stop it: `ssh nas 'kill $(ps -ef | grep "[g]itea web" | awk "{print \$2}")'`

### SSH access to Gitea repositories

Add your public key in Gitea (avatar → **Settings → SSH / GPG Keys**), then add this alias to `~/.ssh/config`. It's already done on the Windows PC.

```
Host gitea
    HostName 192.168.1.105
    Port 2222
    User juren
    IdentityFile ~/.ssh/id_ed25519
```

Clone a repo: `git clone gitea:juren/<name>.git`

Full URL form, for tools that can't use the alias: `ssh://juren@192.168.1.105:2222/juren/<name>.git`

On the **LMDE workstation**, add the same `gitea` block to `~/.ssh/config`. Paste the contents of its `~/.ssh/id_ed25519.pub` into Gitea's SSH Keys page; `ssh-copy-id` doesn't work with Gitea.

### Two kinds of NAS repositories

| | Plain git server | Gitea |
|---|---|---|
| Location | `/volume1/git/<name>.git` | `/volume1/git/gitea/repositories/juren/<name>.git` |
| Create | `ssh nas git init --bare -b main /volume1/git/<name>.git` | Gitea web page: **+ → New Repository** |
| URL | `nas:/volume1/git/<name>.git` | `gitea:juren/<name>.git` |
| Shows in the web interface | No | Yes |

Pushing to a plain repo doesn't update Gitea. A project can push to both with one command:

```bash
git remote add gitea gitea:juren/<name>.git
git remote set-url --add --push origin nas:/volume1/git/<name>.git
git remote set-url --add --push origin gitea:juren/<name>.git
git push                     # updates both; 'git pull' still comes from the NAS repo
```

**Example: HSTL-Hours.** `C:\Users\juren\Documents\HSTL-Hours` pushes to both `/volume1/git/HSTL-Hours.git` and http://192.168.1.105:3000/juren/HSTL-Hours (private).

To bring in a GitHub repo, use **+ → New Migration → GitHub**. Choose **This repository will be a mirror** to have Gitea re-sync it from GitHub on a schedule.

### Upgrading Gitea

```bash
ssh nas
cd /volume1/git/gitea
cp data/gitea.db data/gitea.db.bak-$(date +%Y%m%d)
curl -sSfLO https://github.com/go-gitea/gitea/releases/download/vX.Y.Z/gitea-X.Y.Z-linux-arm64
curl -sSfLO https://github.com/go-gitea/gitea/releases/download/vX.Y.Z/gitea-X.Y.Z-linux-arm64.sha256
sha256sum -c gitea-X.Y.Z-linux-arm64.sha256 && chmod 755 gitea-X.Y.Z-linux-arm64
kill $(ps -ef | grep "[g]itea web" | awk '{print $2}')
ln -sf gitea-X.Y.Z-linux-arm64 gitea && ./start.sh
```

### Access tokens

A token named `claude-setup` (scopes `write:repository`, `write:user`) was created so Claude Code could create the HSTL-Hours repo and add the PC's SSH key through the API. Review or delete it under avatar → **Settings → Applications**.

---

## Troubleshooting (lessons from setup)

| Symptom | Cause / fix |
|---|---|
| `Permission denied (publickey,password)` even with the right password | The user isn't in the `administrators` group, so DSM refuses SSH. Add the user in Control Panel → User & Group → User Groups. |
| Password never prompted; three quick "Permission denied" lines | The command ran without an interactive terminal (for example, Claude Code's `!` prefix). Run password prompts in a normal terminal window. |
| PowerShell: `Unexpected token '\.ssh\id_ed25519.pub'` | `type` was missing at the start of the command. |
| Key installed but still asks for a password | Home or `.ssh` permissions are too open. Run the `chmod` fix above. |
| `src refspec main does not match any` | The local branch is `master`. Push it with `git push origin HEAD:main`, or set `init.defaultBranch main`. |
| `connection is not using a post-quantum key exchange` warning | DSM's SSH server is older. The warning is harmless on the home LAN. |
| `ssh: connect ... port 22: Connection refused` | Port 22 was used instead of 222. Use the `nas` alias, or `-p 222` / `ssh://juren@192.168.1.105:222/volume1/git/<name>.git`. |
| Two-factor authentication | It doesn't apply to SSH on Synology. Only DSM web and app logins use it. |
| Gitea page doesn't load after a NAS reboot | The Boot-up task is missing or disabled. Run `ssh nas /volume1/git/gitea/start.sh` and check Task Scheduler. |
| Gitea slow to respond right after starting | Normal on the DS218's ARM processor; startup takes several seconds. |
| `Permission denied (publickey)` on port 2222 | The key isn't in Gitea's **SSH / GPG Keys** page. DSM's `authorized_keys` isn't used for port 2222. |
| A move fails with "being used by another process" on Windows | LibreOffice (or another app) still has the file open, sometimes with no visible window. Quit the app completely. |

### URL without the SSH config alias

If a tool can't use `~/.ssh/config`, give it the full URL:

```
ssh://juren@192.168.1.105:222/volume1/git/<name>.git
```

---

## Backup note

The repositories are ordinary files under `/volume1/git`. To protect them against drive failure or accidental `rm -rf`, include the `git` shared folder in a **Hyper Backup** task or snapshot schedule. DSM's `#recycle` bin only catches deletions made through DSM file services, not through SSH.

The backup also covers Gitea, because it all lives in `/volume1/git/gitea`: its repositories, database and config. For a consistent copy of the database, back up while Gitea is stopped, or schedule the backup for a quiet time.

---

**Last Updated:** 2026-10-29 2113 CDT
