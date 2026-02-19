## Introduction to Vim

### 1. Introduction — Why Learn Vim?

Vim is not just a text editor.

It is a **modal editing system** designed for speed, precision, and minimal hand movement.

Beginners struggle because Vim behaves very differently from traditional editors.

But once understood, Vim becomes extraordinarily efficient.

---

### 2. The Most Important Concept — Modes

Unlike normal editors, Vim has **modes**.

You are not always typing text.

Primary modes:

**Normal Mode** → Navigation & commands  
**Insert Mode** → Typing text  
**Visual Mode** → Selecting text  
**Command Mode** → Running commands

---

### 3. Starting Vim

Open a file:

	vim filename.txt

If file exists → opens  
If not → creates new file

---

### 4. Normal Mode (Default State)

When Vim opens, you are in **Normal Mode**.

Typing letters does NOT insert text.

Instead, keys perform actions.

This is the biggest beginner confusion.

---

### 5. Switching to Insert Mode

Press:

	i

Now you can type normally.

Return to Normal Mode:

	ESC

ESC is your "home base".

---

### 6. Basic Movement (Essential Skill)

Navigation uses letters:

	h → left  
	j → down  
	k → up  
	l → right  

Yes, intentionally unconventional.

---

### 7. Faster Movement

Move by words:

	w → next word  
	b → previous word  

Move to line boundaries:

	0 → start of line  
	$ → end of line  

---

### 8. Vertical Navigation

Go to line:

	:10

Go to file start/end:

	gg → top  
	G → bottom  

---

### 9. Saving and Quitting

Save:

	:w

Quit:

	:q

Save & quit:

	:wq

Force quit:

	:q!

---

### 10. Deleting Text (First Real Editing Command)

Delete character:

	x

Delete word:

	dw

Delete line:

	dd

Key idea:

> Vim commands are composable

---

### 11. Undo & Redo

Undo:

	u

Redo:

	CTRL + r

---

### 12. Visual Mode (Selecting Text)

Enter Visual Mode:

	v

Select using movement keys.

Delete selection:

	d

Copy selection:

	y

---

### 13. Copy & Paste

Copy (yank):

	yy → copy line  
	yw → copy word  

Paste:

	p

---
