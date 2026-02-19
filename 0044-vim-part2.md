## Vim Movement & Editing

### 1. Introduction — From Movement to Editing Power

Vim’s true strength emerges when you combine:

✔ Operators  
✔ Motions  
✔ Text objects  

This is where Vim becomes fast.

---

### 2. Operators — Actions You Perform

Common operators:

	d → delete  
	y → copy (yank)  
	c → change  
	> → indent  

Operators need targets.

---

### 3. Motions — Define Scope

Examples:

	w → word  
	$ → end of line  
	j → next line  

Combine:

	dw → delete word  
	d$ → delete to end of line  

---

### 4. The Change Operator (Extremely Important)

	cw → change word  
	cc → change line  

Meaning:

✔ Delete target  
✔ Enter Insert Mode automatically  

---

### 5. Repetition (Huge Productivity Feature)

Repeat last command:

	.

Example:

cw hello → change word  
. → repeat change  

---

### 6. Text Objects (Game-Changing Concept)

Operate on logical structures.

Examples:

	diw → delete inside word  
	daw → delete around word  

Inside quotes:

	di" → delete inside quotes  

Inside parentheses:

	di(

---

### 7. Why Text Objects Matter

They allow **semantic editing** instead of character-by-character editing.

Much faster, much cleaner.

---

### 8. Search (Critical Skill)

Search forward:

	/text

Search backward:

	?text

Next match:

	n

Previous match:

	N

---

### 9. Replace & Substitute

Replace character:

	rx

Replace word globally:

	:%s/old/new/g

---

### 10. Line Editing Shortcuts

Join lines:

	J

Duplicate line:

	yy + p

Delete multiple lines:

	5dd

---

### 11. Registers (Hidden Power Feature)

Registers store copied/deleted text.

Paste from register:

	"ap

List registers:

	:reg

---

### 12. Visual Block Mode (Advanced Selection)

Enter:

	CTRL + v

Useful for:

✔ Column editing  
✔ Multi-line modifications  

---

### 13. Indentation & Formatting

Indent line:

	>>

Indent selection:

	> (in visual mode)

Auto-indent file:

	gg=G
