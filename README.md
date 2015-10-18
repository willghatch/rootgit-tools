rootgit
=======

Rootgit is a format I've made to handle system configuration.

For quite a while now I've kept my user configuration in a git repository
and kept things synced with a little script.  Some time later, I decided
my system configurations deserved the same, at which point I did
various ad-hoc linking into git repositories.  Then I decided to standardize
what I was doing.

The Convention
--------------

The script `rootgit-linkup` does symlinking (and moves any existing
files that it replaces in `$ROOTGIT/overwrites.rootgit/`.  IE the rootgit
mirrors the root directory structure, and all files in the root that
also has a corresponding file in the rootgit will be a symlink into the
rootgit.  Files in a `.git` directory or with a name or path that
includes `.rootgit` are ignored when linking.

The script `rootgit-run-scripts` runs all executables in the rootgit
named `*.rootgit`.  The motivation of this is to be able to have scripts
to set ownership and permission of the files, which can be run any time
the git repository is updated.  In other words, since git only preserves
the executable bit of Unix permissions and all the files in the rootgit
will originally be owned by root, yet you may want files in /etc or
such to be owned by an app user or have different permissions (eg private
keys or sensitive data), you can add these scripts to keep those
permissions even when the git repo messes them up.

Each `*.rootgit` script runs with a `$PWD` of the directory it's in,
so you can `chown`/`chmod` directly without absolute paths.

I've made the scripts work using relative links, and needing a path
to the root so it can also work for chroot environments.

Why
---

I wanted a clean, standard way to manage this.  I know there are big tools for
managing server configs, but really I just want the simplicity of files in
a git repo, because I'm only doing this for a few computers that have
different purposes.

If you find it useful or want to improve it, have at it!

Todo
----

- Configurable name transformers.  For example, a lot of files need to start
  with a dot in the real file hierarchy, but I want them not to be hidden
  in the rootgit.  So I want to be able to say "DOT" or something will be
  translated to "." in the real tree.
- This and perhaps other per-rootgit configuration.  It should go in
  `$ROOTGIT/conf.rootgit`.

License
-------

MIT license -- do what you want, don't blame me for problems.
