---
date: April 30, 2018
---

# `sleep infinity`

## Fun Fact

`sleep` is a useful POSIX function declared in `<unistd.h>`. It is also
provided as a higher level to the user. The `sleep` command is commonly used to
delay other commands, for instance in shell scripts.

The GNU coreutils `sleep` command can delay for an infinite amount of time as
follows:

```bash
sleep infinity
```

## Explanation

This *feature* is actually a consequence of how the arguments are parsed.

1. In [`sleep` source code](https://github.com/coreutils/coreutils/blob/22424dd/src/sleep.c#L132):
   each `char*` argument is converted to `DOUBLE` using
   [`xstrtod` function](https://github.com/coreutils/gnulib/blob/c66dba9/lib/xstrtod.h)
   from gnulib.
1. In [`xstrtod` source code](https://github.com/coreutils/gnulib/blob/c66dba9/lib/xstrtod.c#L52):
   a `convert` function parameter is called. At runtime,
   [`xstrtod` callee](https://github.com/coreutils/coreutils/blob/22424dd/src/sleep.c#L132)
   uses [`c_strtod` function](https://github.com/coreutils/gnulib/blob/c66dba9/lib/c-strtod.h).
1. In [`c_strtod` source code](https://github.com/coreutils/gnulib/blob/c66dba9/lib/c-strtod.c):
   `STRTOD` ( or `STRTOD_L`, depending on
   [compilation flags](https://github.com/coreutils/gnulib/blob/c66dba9/lib/c-strtod.c#L70) )
   is called, and this function somehow maps to
   [`strtod` function](https://github.com/coreutils/gnulib/blob/c66dba9/lib/strtod.c#L205-L346).
1. In [`strtod` source code](https://github.com/coreutils/gnulib/blob/c66dba9/lib/strtod.c#L294-L308):
   there is an actual check for `infinity` keyword ( along with a
   [check for `nan` keyword](https://github.com/coreutils/gnulib/blob/c66dba9/lib/strtod.c#L309-L331) ).
   In such case, the returned parsed number is then
   [`HUGE_VAL`](https://github.com/coreutils/gnulib/blob/c66dba9/lib/strtod.c#L306).

As an other consequence, this very source code provides a useful short writing:

```bash
sleep inf
```

## References

- [Linux Programmer's Manual](http://man7.org/linux/man-pages/man3/sleep.3.html)
- [GNU Documentation](https://www.gnu.org/software/coreutils/sleep)
- [Stack Overflow](https://stackoverflow.com/a/22100106)
- [First time I saw it in action](https://zwischenzugs.com/2018/04/23/unprivileged-docker-builds-a-proof-of-concept/)
