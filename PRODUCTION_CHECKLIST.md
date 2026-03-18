# Production Checklist

Use this checklist before declaring the product "production-ready".

## Security

- [ ] `JWT_SECRET` is unique and stored securely.
- [ ] `CORS_ALLOWED_ORIGINS` contains only trusted origins.
- [ ] Default demo credentials are removed or rotated.
- [ ] Sensitive environment variables are not committed to git.
- [ ] Electron external links are restricted to allowed protocols.
- [ ] Electron `contextIsolation` is enabled.
- [ ] Electron `nodeIntegration` is disabled.
- [ ] Electron preload exposes only required APIs.

## Backend

- [ ] `DB_TYPE` is set correctly for the target environment.
- [ ] `DB_SYNCHRONIZE` is disabled for production.
- [ ] Database backups are configured and tested.
- [ ] `/api/get-status` responds reliably.
- [ ] Error responses do not leak internal implementation details.
- [ ] Auth endpoints (`/api/auth/login`, `/api/auth/register`) are validated and tested.
- [ ] Protected endpoints require valid JWT.

## Frontend

- [ ] Default route opens login page.
- [ ] Unauthorized access to protected pages is blocked.
- [ ] Login, register, logout flows are verified.
- [ ] User-facing error messages are clear and actionable.
- [ ] App branding is consistent (`White Fox` title and labels).

## Desktop Runtime

- [ ] Electron starts backend and waits for readiness before rendering.
- [ ] Runtime versions (`Node`, `Chrome`, `Electron`, `NestJS`, `Angular`) are displayed correctly.
- [ ] Backend process terminates on app shutdown in dev and packaged modes.
- [ ] Packaged app launches without missing preload/main path errors.

## Quality Gates

- [ ] `npm run lint` passes.
- [ ] `npm run test` passes.
- [ ] `npm run e2e` passes.
- [ ] `npm run e2e:packaged` passes.
- [ ] Manual smoke test on target OS is complete.

## Compliance and Documentation

- [ ] `LICENSE.md` reflects current commercial terms.
- [ ] `README.md` matches real commands and behavior.
- [ ] Environment variable documentation is up to date.
- [ ] Support/contact information is valid.
