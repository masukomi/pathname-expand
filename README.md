## Pathname expansion

Shell-style homedir expansion in path names.

This egg serves as a partial replacement of the historical expansion
behaviour of all file operations in CHICKEN core: strings prefixed
with a tilde are expanded into home directories, but strings prefixed
with a dollar sign are not expanded into environment variables.

### Author

The CHICKEN Team

### Requirements

None

### Documentation

```
[procedure] (pathname-expand STRING #!optional BASE)
```

If `STRING` is identical to `"~"` or if it begins with `"~/"`,
return the argument with the `"~"` substituted by the current user's
home directory.  If the current user cannot be determined or does not
exist, a condition of type `(exn pathname-expand username)` is
raised.

If `STRING` begins with `"~USERNAME"`, return the argument with
the `"~USERNAME"` substituted by the provided user's HOME directory.
On Windows, this will be the value of the environment variables
`USERPROFILE` or `HOME` (or `"."`  if none of the variables is
set). On Unix systems, the user database is consulted.  If the user
does not exist, a condition of type `(exn pathname-expand username)`
is raised.

If `STRING` is identical to `"~~"` or if it begins with `"~~/"`,
return the argument with the `"~~"` substituted by
`(repository-path)`.  If there is no repository path, a condition
of type `(exn pathname-expand repository-path)` is raised.

If `STRING` doesn't begin with a tilde, and it represents an
absolute pathname, then it is returned unchanged. If instead it is a
relative pathname the result of `(make-pathname STRING BASE)` is
returned, where `BASE` defaults to the current working directory.

#### Known bugs and limitations

The concept of pathname expansion is fuzzy and ill-defined.  It boils
down to "do what I mean".  Because of this, in situations where
security is a concern this egg should be used with great care.

Furthermore, the exact expansion rules differ across shells, and it's
unclear exactly how it should behave under non-UNIX operating systems.

### Changelog

* 0.1 Initial release
* 0.2 Modified for Chicken 5

