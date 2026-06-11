---
title: Vim
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
## To open a buffer of a file:

```
:e /path/filename.txt
```

## To open a new buffer with the existing file content:

```
:badd <existing filename>
```

## To close the buffer:

```
:bd
```

## To move to next buffer

```
:bn
```

## To move to previous buffer

```
:bp
```

## To open an empty buffer

```
:enew
```

## To horizontal split

```
:split /path/filename
```

## To vertical split

```
:vsplit /path/filename
```

## To open a single file in split

```
:vsplit <path/samefilename_that_is_opened_currently>
```

## To cancel the split window of selected one :

```
ctrl + w , then press q
```

## To move to next split :

```
ctrl + ww
```

## Insert content of other files in current vim editing file:

```
:r <filename to insert the content>
```

## To go to top of the file

```
gg
```

## To go to bottom of the file

```
Shift + g
```

## Undo

```
u
```

## Redo

```
ctrl + r
```

## Visual mode for selection

```
v
```

## Copy the text

```
v
[select the text]
y - to copy the selected text
```

## Paste the selected text

```
p
```

## Cut the text

```
v
[select the text]
d - to cut the selected text
```

## Navigate to end of the line

```
end
```

## Navigate to starting of the line

```
home
```

## Find and replace

```
%s/<foo>/<bar>/g
// replace the foo with the bar text
```

## Cut ant paste using dd trick

```
dd - to cut the line
p  - to paste the line
```

## Copy line

```
yy
```

## Open vim window with horizontal split

```
# vim -o <file1> <file2>
```

## Open vim window with vertical split

```
# vim -O <file1> <file2>
```

## Replay previous command

```jsx
. <dot>
```

## Navigate block by block wise

```jsx
{
}
```

---

# Configure Vim

---

## Add line number

```
:set number
```

## Remove line number

```
:set nonumber
```

## Make .vimrc file in home directory

```
"Add line number"
set number
```

