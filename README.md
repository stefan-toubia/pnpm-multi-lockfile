# pnpm repro

Issues with `pnpm install` and `recursive-install=false`

1. Project not found when running `pnpm install` inside a project directory.

    ```sh
    $ cd project-b/src
    $ pnpm install
    No projects matched the filters in "/Users/stoubia/repos/pnpm-multi-lockfile"
    ```

    Expected: pnpm should find the project root by walking up the directory tree.

2. `--dir`/`-C` does not work for `pnpm install`.

    [Repro script](./project-b/bin/install2.sh)

    ```sh
    $ cd project-b/src
    $ pnpm --dir .. install
    No projects matched the filters in "/Users/stoubia/repos/pnpm-multi-lockfile"
    ```

    Expected: pnpm should run the install command from the specified directory.

3. pnpm exits with 0 exit code when project not found

    ```sh
    $ cd project-b/src
    $ pnpm install
    No projects matched the filters in "/Users/stoubia/repos/pnpm-multi-lockfile"
    $ echo $?
    0
    ```

    Expected: pnpm should output a non-zero exit code.
