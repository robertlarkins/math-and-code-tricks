# README

This area is for issues and errors that have been observed and the approaches taken to solve them.

## General Issues

### `file not found`

If a computer, compiler, error message, etc. says `file not found`, then trust that it can't find the file, regardless of what you believe is true.
What the computer is saying is _I can't find or access a file with exactly this name (or thereabouts) at the precise location where I (the computer) and not you (the human) think I have been instructed to look for it_.

Too often we argue (unsuccessfully) with computers based on what **we** think the facts are, rather than asking _what does the **computer** think the facts are_.
Asking this helps us get to the root of the problem faster.

So review the following:
- Where is the computer actually looking for the file
  - Is the file actually there?
  - What is the file path, is it using forward or backslashes (backslashes wont work in Linux).
  - Is there a trailing slash that should or should not be there?
  - Is the computer looking for the file in different locations based on a search order. This will more likely result in the system thinking it has found the file, but not operating the way you expect. In this situation see if there is documentation that answers this.
- Does the file name match what the system expects? Taking into consideration the environment (Windows is case insensitive, while Linux is case sensitive).
- Does the system trying to access the file have permission to access it?

The outcome of the computer/system performing this operation will liekly differ between environments. So review error messages and logging to help diagnose this.
