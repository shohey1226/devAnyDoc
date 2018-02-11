---
date: 2016-03-09T00:11:02+01:00
title: How to use terminal
weight: 10
---

Terminal is a terminal session to connect to server. You can use [screen](https://www.gnu.org/software/screen/)/[tmux](https://github.com/tmux/tmux/wiki) to keep sessions in the background because when devAny app is inacitve(open the other app, for example), it cuts the sesesson.

## devany command 

`devany` command is available and this command opens file or link to Editor and Browser respectively.

<video src="/movies/terminal_command.mp4" controls width="100%" autoplay></video>

### Open file with Editor from Terminal

```bash
 $ devany -f content/how-to-use/index.md
```

---

### Open link with Browser from Terminal

```bash
 $ devany -l https://www.google.com 
```

---

## How to copy text

1. Find word with command+f
2. Type word and enter to hightlight
3. Type "i" to go in Insert mode
4. Use arrow keys to select the area
5. Type enter to copy the words to clipboard
6. Then, you can paste with command+v on Terminal, Edtior or Browser 

<video src="/movies/terminal_copy.mp4" controls width="100%"></video>

## Shortcuts

| Keyboard Command   | Description 
| --- | --- 
| Command + f  | Search a word
| Command + c | Copy
| Command + v | Paste  
| Command + r | Reload _*Try when Terminal is hung_

