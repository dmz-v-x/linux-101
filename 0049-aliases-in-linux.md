## Introduction to aliases in Linux

### 1. Introduction — What Are Aliases in Linux?

Aliases are one of the simplest yet most powerful customization features in Linux.

They allow you to:

✔ Rename commands  
✔ Create shortcuts  
✔ Build command templates  
✔ Improve typing efficiency  

In essence:

> An alias is a user-defined shortcut for a command.

---

## PART 1 — The Core Idea of Aliases

---

### 2. Why Aliases Exist

Linux commands can become long:

	ls -alh
	ssh user@host -i ~/.ssh/key -p 2222
	docker container ls --format ...

Aliases reduce friction.

Example goal:

✔ Less typing  
✔ Faster workflows  
✔ Fewer mistakes  

---

### 3. Basic Alias Syntax

Command:

	alias name='actual command'

Example:

	alias lh='ls -alh'

Meaning:

✔ `lh` now runs `ls -alh`

---

### 4. Important Syntax Rules

✔ No spaces around `=`  
✔ Command must be quoted  
✔ Quotes preserve full command  

Correct:

	alias ll='ls -l'

Incorrect:

	alias ll = ls -l ❌

---

---

## PART 2 — Session vs Persistent Aliases

---

### 5. Temporary Aliases (Session Only)

When you create:

	alias lh='ls -alh'

It exists only:

✔ In current shell session

Lost when:

✘ Terminal closes  
✘ New session starts  

---

### 6. Why This Happens

Aliases live in:

✔ Shell memory  
✔ Not automatically saved  

---

### 7. Viewing Existing Aliases

Command:

	alias

Displays:

✔ All active aliases

Useful for:

✔ Debugging  
✔ Inspecting defaults  

---

### 8. Removing an Alias

Command:

	unalias lh

Effect:

✔ Alias deleted from session

---

---

## PART 3 — Creating Permanent Aliases

---

### 9. Where Should Aliases Be Stored?

For Bash users:

	~/.bashrc

For Zsh users:

	~/.zshrc

---

### 10. Making Alias Persistent

Append alias:

	echo "alias lh='ls -alh'" >> ~/.bashrc

Reload config:

	source ~/.bashrc

Now alias survives:

✔ Terminal restarts  
✔ New sessions  

---

### 11. Why `source` is Needed

Without sourcing:

✔ Changes apply next login only

With sourcing:

✔ Immediate effect

---

---

## PART 4 — Practical Alias Use Cases

---

### 12. Common Productivity Aliases

	alias ll='ls -l'
	alias la='ls -a'
	alias l='ls -CF'

---

### 13. Safety Aliases (Very Popular)

Prevent mistakes:

	alias rm='rm -i'
	alias cp='cp -i'
	alias mv='mv -i'

Effect:

✔ Confirmation before overwrite/delete

---

### 14. Command Correction Aliases

Example:

	alias cls='clear'

---

### 15. Complex Command Templates

Example:

	alias myip='curl ifconfig.me'

Example:

	alias ports='ss -tuln'

---

---

## PART 5 — Advanced Alias Techniques

---

### 16. Aliases with Arguments (Important Limitation)

Aliases do **NOT** handle dynamic parameters well.

Example:

	alias greet='echo Hello'

Works.

But:

	alias greet='echo Hello $1' ❌

Fails.

Why?

✔ Aliases are simple text substitution

---

### 17. Use Functions Instead (Advanced Solution)

Better approach:

	greet() {
	    echo "Hello $1"
	}

Stored in `.bashrc`.

---

### 18. Aliases vs Functions (Key Difference)

**Alias**

✔ Simple substitution  
✔ No logic  

**Function**

✔ Accepts arguments  
✔ Supports logic  
✔ More powerful  

---

### 19. Chaining Commands

Example:

	alias update='sudo apt update && sudo apt upgrade'

---

### 20. Overriding Existing Commands

Example:

	alias ls='ls --color=auto'

Be cautious:

✔ Can change system behavior  
✔ May confuse scripts  

---

---

## PART 6 — Global Aliases (All Users)

---

### 21. User-Level vs System-Level

So far:

✔ Aliases apply to current user only

For all users:

✔ Use global shell configuration

---

### 22. Proper Global Alias Method

Best practice:

Create file:

	sudo nano /etc/profile.d/custom_aliases.sh

Add:

	alias lh2='ls -alh'

---

### 23. Why `/etc/profile.d/` is Preferred

✔ Clean modular design  
✔ Avoid editing large system files  
✔ Easier maintenance  

---

### 24. Avoid Directly Editing `/etc/profile`

Technically possible, but discouraged.

---

---

## PART 7 — Debugging Alias Issues

---

### 25. Check If Alias Exists

	alias | grep lh

---

### 26. Determine If Command is Alias

Command:

	type lh

Output examples:

	lh is aliased to `ls -alh`

or

	lh not found

---

### 27. Bypassing an Alias

Use backslash:

	\ls

Runs original command.

Useful when:

✔ Alias interferes  
✔ Testing behavior  

---

---

## PART 8 — Important Gotchas

---

### 28. Aliases Only Work in Interactive Shells

Scripts usually ignore aliases unless explicitly enabled.

---

### 29. Aliases Are Shell-Specific

✔ Bash aliases ≠ Zsh aliases

---

### 30. Aliases Are Not Universal Linux Features

✔ Feature of the shell, not kernel

---

---

## PART 9 — Mental Model of Aliases

---

### 31. What Aliases Really Are

Aliases are:

✔ Text replacement rules

Example:

lh → ls -alh

Nothing magical.

No execution logic.

---

### 32. When to Use Aliases

✔ Shortcuts  
✔ Frequently used commands  
✔ Static command templates  

---

### 33. When NOT to Use Aliases

✘ Complex logic  
✘ Dynamic parameters  
✘ Conditional workflows  

Use functions instead.

---

### 34. Final Thoughts

Aliases are one of the fastest ways to improve command-line efficiency.

Small customization → Huge long-term productivity gain.

Mastering aliases means:

✔ Faster typing  
✔ Cleaner workflows  
✔ Fewer repetitive keystrokes  
✔ Personalized Linux environment
