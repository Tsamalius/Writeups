# OverTheWire — Bandit, levels 0 to 5

Environment: Windows desktop, PowerShell, built-in `ssh`.

```
ssh banditN@bandit.labs.overthewire.org -p 2220
```

Passwords are not published — publishing them breaks the game for others.

---

## 0 → 1 : first contact

The password is in a plain file in the home directory.
`ls` to see it, `cat` to read it.

**Learned:** the whole game is `ls`, `cat`, and reading carefully.

---

## 1 → 2 : a file named `-`

`cat -` does not work. It hangs, waiting for input.

```bash
cat ./-
```

**Why:** most Unix tools treat a leading `-` as the start of a flag.
`./` forces it to be read as a path.

**Alternative:** `cat < -` — redirection is handled by the shell, not by `cat`.

---

## 2 → 3 : spaces in the filename

The shell splits arguments on spaces, so the name arrives as several
separate arguments.

```bash
cat "spaces in this filename"
```

**Three ways to solve it:** quotes, backslash escaping, or Tab completion.
Tab is the one you actually use in real work.

---

## 3 → 4 : a hidden file

`ls` in `inhere/` shows an empty directory. It is not empty.

```bash
ls -A inhere/
```

**Why:** a leading dot marks a file as hidden by convention.
This is not a security feature — it hides nothing from anyone who looks.

**`-a` vs `-A`:** `-a` also prints `.` and `..`. `-A` skips them.

---

## 4 → 5 : one readable file out of ten

Ten files, named `-file00` to `-file09`. Nine contain binary data,
one contains the password.

**What I did first:** opened them one by one with `cat ./-file0N`
until one printed readable text. It worked, but it does not scale.
With a hundred files this approach is useless.

**What I should have done:**

```bash
file ./*
```

`file` reads the beginning of each file and reports its type.
Nine return `data`, one returns `ASCII text`.

**Lesson — the real one:** when filtering many candidates, do not test
them one at a time. Find the attribute that separates them, and let a
tool apply it to all of them at once.

This is the same reasoning as recon: you do not connect to 200 open
ports manually. You fingerprint them and read the differences.

**Also note:** the `./` prefix is needed again here — the filenames
start with a dash.
