# Install bin race

There seems to be a race condition on pnpm install when projects define `bin`
scripts. Sometimes this only results in a warning, but sometimes installs fail.

```sh
$ pnpm i
Scope: all 4 workspace projects
Lockfile is up-to-date, resolution step is skipped
Packages: +380
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Packages are hard linked from the content-addressable store to the virtual store.
  Content-addressable store is at: /Users/stoubia/Library/Caches/pnpm/store/v3
  Virtual store is at:             node_modules/.pnpm
 WARN  Failed to create bin at /Users/stoubia/repos/pnpm-multi-lockfile/project-a/node_modules/.bin/eslint. The source file at /Users/stoubia/repos/pnpm-multi-lockfile/common/tool-a/node_modules/.bin/eslint does not exist.
 WARN  Failed to create bin at /Users/stoubia/repos/pnpm-multi-lockfile/project-a/node_modules/.bin/stylelint. The source file at /Users/stoubia/repos/pnpm-multi-lockfile/common/tool-b/node_modules/.bin/stylelint does not exist.
Progress: resolved 380, reused 380, downloaded 0, added 380, done
```

Second install (hard to reproduce):

```sh
$ pnpm i
Scope: all 4 workspace projects
Lockfile is up-to-date, resolution step is skipped
Already up-to-date
 ENOENT  ENOENT: no such file or directory, chmod '/Users/stoubia/repos/pnpm-multi-lockfile/common/tool-a/node_modules/.bin/eslint'
```

Third install:

```sh
$ pnpm i
Scope: all 4 workspace projects
Lockfile is up-to-date, resolution step is skipped
Already up-to-date
```

## Repro steps

Delete all `node_module` directories

    # From the project root
    npx rimraf ./**/node_modules

Install all, you should get a warning like the first `pnpm i` run above.

    pnpm install

Run `pnpm i` again, you won't get a warning but sometimes install will fail with
second error above.


Install failures are very common when switching between branches.

Example:

```sh
~/repos/pnpm-multi-lockfile on  master ⌚ 12:45:07
$ pnpm i
Scope: all 4 workspace projects
Lockfile is up-to-date, resolution step is skipped
Already up-to-date
 WARN  Failed to create bin at /Users/stoubia/repos/pnpm-multi-lockfile/common/tool-b/node_modules/.bin/eslint. The source file at /Users/stoubia/repos/pnpm-multi-lockfile/common/tool-a/node_modules/.bin/eslint does not exist.
 ENOENT  ENOENT: no such file or directory, chmod '/Users/stoubia/repos/pnpm-multi-lockfile/common/tool-a/node_modules/.bin/eslint'



~/repos/pnpm-multi-lockfile on  install-race ⌚ 12:45:13
$ pnpm i
Scope: all 4 workspace projects
Lockfile is up-to-date, resolution step is skipped
Already up-to-date
 WARN  Failed to create bin at /Users/stoubia/repos/pnpm-multi-lockfile/common/tool-b/node_modules/.bin/eslint. The source file at /Users/stoubia/repos/pnpm-multi-lockfile/common/tool-a/node_modules/.bin/eslint does not exist.

~/repos/pnpm-multi-lockfile on  install-race ⌚ 12:45:29
$ pnpm i
Scope: all 4 workspace projects
Lockfile is up-to-date, resolution step is skipped
Already up-to-date

~/repos/pnpm-multi-lockfile on  install-race ⌚ 12:45:33
```

Similar results:

```sh

~/repos/pnpm-multi-lockfile on  install-race ⌚ 12:45:33
$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

~/repos/pnpm-multi-lockfile on  master ⌚ 12:46:26
$ pnpm i
Scope: all 2 workspace projects
project-b/node_modules/.pnpm/gifsicle@7.0.1/node_modules/gifsicle: Running postinstall script, done in 217ms

~/repos/pnpm-multi-lockfile on  master ⌚ 12:46:29
$ git checkout install-race
Switched to branch 'install-race'

~/repos/pnpm-multi-lockfile on  install-race ⌚ 12:46:36
$ pnpm i
Scope: all 4 workspace projects
Lockfile is up-to-date, resolution step is skipped
Already up-to-date
 ENOENT  ENOENT: no such file or directory, chmod '/Users/stoubia/repos/pnpm-multi-lockfile/common/tool-a/node_modules/.bin/eslint'



~/repos/pnpm-multi-lockfile on  install-race ⌚ 12:46:38
$ pnpm i
Scope: all 4 workspace projects
Lockfile is up-to-date, resolution step is skipped
Already up-to-date
```
