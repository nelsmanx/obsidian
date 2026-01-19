
## TLDR 

Simplified, example based and community-driven `man` pages. `Tealdeer` is a very fast implementation of `tldr` in Rust.

```bash
$ sudo apt install tealdeer
$ tldr -u | proxychains4 tldr -u   # update the local cache
$ tldr ls                          # usage: tldr [COMMAND]
```

Если `tldr -u`  не срабатывает, можно вручную воссаздать структуру  

```
.cache
├── tealdeer
│   └── tldr-pages
│       ├── LICENSE.md
│       ├── index.json
│       ├── pages
│       ├── pages.en
```
___

