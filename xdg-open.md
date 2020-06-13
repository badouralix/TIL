---
date: June 17, 2018
---

# `xdg-open`

## Fun fact

One key command in macos is in my opinion `open`, which opens a file ( or URL ) passed as argument in the user's preferred application or default browser if a URL is passed.

It turns out linux also provides a similar tool as part of the `xdg-utils` set of tools :

```bash
xdg-open FILE
xdg-open URL
```

Even though the option flags are quite different, this alias turns out to be quite useful :

```bash
alias open='xdg-open'
```

## References

- [Linux documentation for `xdg-open`](https://linux.die.net/man/1/xdg-open)
- [The macos `open` command](https://scriptingosx.com/2017/02/the-macos-open-command/)
- [`xdg-utils` homepage](https://www.freedesktop.org/wiki/Software/xdg-utils/)
