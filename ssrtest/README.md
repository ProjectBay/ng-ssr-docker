# Angular Docker SSR V17 Issue

Hello.

This repository displays an angular ssr docker issue.

When trying to run npx run start inside docker to serve the angular ssr app, docker can't expose the ports. Therefore the application will be unavailable.

The workaround for this issue is documented inside the README of the "fix" branch of this repository.

## How to test the issue

- Node: 18.14.0
- Package Manager: PNPM (not tested with other pm's)

Make sure to have docker installed.

1. `pnpm install`
2. `pnpm run docker-build`
3. `pnpm run docker-run`

localhost:4200 is now unreachable.

To see a working workaround, checkout the "fix" branch, run pnpm install - build & run the docker image again.
