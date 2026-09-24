# 06 · Sidekicks — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent five sessions on it: finding out what
went wrong, then building the fix nobody had — a working prototype,
by hand, in one sitting, for one room.

At the end of the session, ask Claude Code to save the prompts you
wrote yourself below — not the starter prompt. The closing slide has
the exact prompt to paste. By Module 6 this file is a prompt library
built from your own questions.

---

### 1.

```
I want you to create a skill, to review a brief before it goes any further, here is what I actually check for:

* It should have the owner name
* Date and time when this was creater
```

### 2.

```
Stop
```

### 3.

```
want you to create a skill, to review a brief before it goes any further, here is what I actually check for:

* It should have the owner name
* Date and time when this was creater

* It mentions how we'll know it worked
* The scope at the end mataches the scope at the start
* It explains the problem before it propose a fix


Turn that into a skill called review-checklist, so I can point it to a document
```

### 4.

```
Can run this skill for my previous brief document that i created
```

### 5.

```
Read the brief_matt2.md file from my documenta folder and run my review-checklist skill
```

### 6.

```
You are an expert in creating the brief document, review the issues found in my brief.md file and fix it.
```

### 7.

```
stop
```

### 8.

```
You are an expert in creating the brief document, review the issues found in my 05-super-speed folder and fix it.
```

### 9.

```
schedule to run review-checklist on all the *brief*.md file under repos folder on Monday morning 8:00 AM Eastern time
```

### 10.

```
Can you run now just so I know it is working, run it 2 twice and then stop
```

### 11.

```
create a new skill for this and name it save-pm-work
```
