# TODO

Last investigation: 2026-10-17. Compared this collection with the backend in `../spring-boot`
(`spring-boot-maven-sample/backend`, branch `master`, at `dbe08a2`).

Work items top to bottom, one per commit. When an item is fixed, move it to **Done** with its
commit hash. Items that need a human decision go in **Needs you**.

## Needs you

Nothing open.

## Done

- [x] Expired dev JWTs and the old `devpassword` stay in git history: the dev `JWT_SECRET` and that password were
      only ever used locally, so no rotation, and history isn't rewritten (it's pushed; a rewrite would un-leak nothing)

All in one commit (d03caad):

- [x] Removed `DebugToken.yml` and `DebugToken Validate.yml`: `/debug/*` no longer exists and HTTP Basic is disabled
- [x] Delete Expense sends `?version={{expenseVersion}}`, which the backend requires
- [x] Update Archived Expense uses the collection token instead of a hardcoded, expired JWT
- [x] Login reads `{{username}}` and the secret `{{password}}` from the environment instead of `admin`/`devpassword`
- [x] Get Expenses no longer has an empty query param
- [x] `baseUrl` in `environments/dev.yml`; every request uses `{{baseUrl}}`. No second environment: Docker Compose
      also listens on `localhost:8080`
- [x] `refreshToken` declared in `environments/dev.yml`; `password`, `jwtToken`, `refreshToken` and `newPassword` are secret
- [x] No hardcoded ids: Create/Update/Patch store `expenseId`/`expenseVersion`, Delete and Get Archived store
      `archivedExpenseId`, Register/List Users store `userId`, the session lists store session ids
- [x] Bearer `{{jwtToken}}` set once in `opencollection.yml`; requests use `auth: inherit` (Health uses none)
- [x] Added Register and Logout (Logout clears the tokens)
- [x] Added `/users/me`: Get Me, List My Sessions, Revoke My Session, Revoke Other Sessions, Change Password
- [x] Added admin: List Users, Get User, Update User, List User Sessions, Revoke User Session, Revoke User Sessions
- [x] Added `GET /actuator/health`
- [x] Requests grouped in `auth/`, `expense/`, `users-me/`, `admin/`, `actuator/` folders, renumbered in run order
- [x] API versioning: Get Expenses (1.1) and Get Expenses v1.0; the header is dropped where the version makes no difference
- [x] README explains starting the backend, the environment, the Login-first flow and the folders
- [x] Decided the admin password lives in the secret `password` variable, set locally by each user
