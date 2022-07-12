# Multi-lockfile pnpm project

Project for reproducing issues with the side-effect caching that causes
lifecycle scripts to always be run during `pnpm install`.

## Repro steps

1. Install this project

    pnpm i

2. Run install a second time, note that the gifsicle postinstall script has run.

3. Enable shared lockfile in `.npmrc`.

4. Run install to generate the shared lockfile.

5. Run install again, note that lifecycle scripts have not run.

```sh
# Step 1
~/src/pnpm-multi-lockfile on  master! ⌚ 11:53:42
$ pnpm i
Scope: all 2 workspace projects
project-b                                | +171 +++++++++++++++++
Packages are hard linked from the content-addressable store to the virtual store.
  Content-addressable store is at: /Users/stoubia/Library/pnpm/store/v3
  Virtual store is at:             project-a/node_modules/.pnpm
project-b                                | Progress: resolved 171, reused 171, downloaded 0, added 171, done
project-b/node_modules/.pnpm/gifsicle@7.0.1/node_modules/gifsicle: Running postinstall script, done in 556ms

# Step 2
~/src/pnpm-multi-lockfile on  master! ⌚ 11:53:46
$ pnpm i
Scope: all 2 workspace projects
project-b/node_modules/.pnpm/gifsicle@7.0.1/node_modules/gifsicle: Running postinstall script, done in 235ms

# Step 4
~/src/pnpm-multi-lockfile on  master! ⌚ 11:55:25
$ pnpm i
Scope: all 2 workspace projects
project-b                                |  WARN  deprecated uuid@3.4.0
Packages: +171
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Packages are hard linked from the content-addressable store to the virtual store.
  Content-addressable store is at: /Users/stoubia/Library/pnpm/store/v3
  Virtual store is at:             node_modules/.pnpm
Progress: resolved 209, reused 171, downloaded 0, added 171, done

# Step 5
~/src/pnpm-multi-lockfile on  master! ⌚ 11:55:40
$ pnpm i
Scope: all 2 workspace projects
Lockfile is up-to-date, resolution step is skipped
Already up-to-date

~/src/pnpm-multi-lockfile on  master! ⌚ 11:58:12
$ pnpm i
Scope: all 2 workspace projects
Lockfile is up-to-date, resolution step is skipped
Already up-to-date
```
