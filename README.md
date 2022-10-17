# pnpm repro

`pnpm --silent install` does not show an error message on error.

Repro:

`project-b` package defines an invalid dependency `package-does-not-exist`

```sh
$ cd project-b
$ pnpm --silent install

$ pnpm install
 ERR_PNPM_FETCH_404  GET https://registry.npmjs.org/package-does-not-exist: Not Found - 404

package-does-not-exist is not in the npm registry, or you have no permission to fetch it.

No authorization header was set for the request.
Progress: resolved 0, reused 1, downloaded 0, added 0
```
