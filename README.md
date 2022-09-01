# pnpm repro

Scripts are being skipped when --if-present is passed

## Repro

```sh
$ cd project-a

$ pnpm tsc
src/index.ts:4:6 - error TS2304: Cannot find name 'Unreferenced'.

4   a: Unreferenced;
       ~~~~~~~~~~~~


Found 1 error in src/index.ts:4

$ pnpm run test

> project-a@1.0.0 test /Users/stoubia/repos/pnpm-multi-lockfile/project-a
> pnpm tsc && echo "Test 2"

src/index.ts:4:6 - error TS2304: Cannot find name 'Unreferenced'.

4   a: Unreferenced;
       ~~~~~~~~~~~~


Found 1 error in src/index.ts:4

 ELIFECYCLE  Test failed. See above for more details.

$ pnpm run --if-present test

> project-a@1.0.0 test /Users/stoubia/repos/pnpm-multi-lockfile/project-a
> pnpm tsc && echo "Test 2"

Test 2
```
