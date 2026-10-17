🐶
Bruno collection for [spring-boot-maven-sample](https://github.com/ezekielbaniaga/spring-boot/tree/master/spring-boot-maven-sample)

## Start the backend

From the `spring-boot` repo, either:

- `spring-boot-maven-sample/backend/run_postgres_container.sh`, then `spring-boot-maven-sample/run_project.sh`
  (the `dev` profile), or
- Docker Compose: in `spring-boot-maven-sample/docker_compose`, `cp .env.example .env`, set the secrets, then
  `./run_compose.sh`.

Both listen on `http://localhost:8080`. The backend needs `ADMIN_PASSWORD` (at least 12 characters) and
`JWT_SECRET` (at least 32 bytes).

## Use the collection

1. Open this folder in Bruno and select the `dev` environment.
2. Set the secret `password` to the backend's `ADMIN_PASSWORD`. Secret values stay on your machine and are
   never written to `environments/dev.yml`.
3. Run **auth/Login**. It stores `jwtToken` and `refreshToken`; every other request sends
   `Bearer {{jwtToken}}`, set once in the collection settings. Run **auth/Refresh** when the access token
   expires (15 minutes).

Requests store the ids they return for the next ones: **expense/Create Expense** sets `expenseId` and
`expenseVersion` for Find, Update, Patch and Delete; **auth/Register** or **admin/List Users** sets `userId`
for the admin requests. **auth/Register** and **users-me/Change Password** need the secret `newPassword`.

| Folder     | Backend                 | Who                      |
|------------|-------------------------|--------------------------|
| `auth`     | `/auth/*`               | anyone                   |
| `expense`  | `/api/expense/**`       | any logged-in user       |
| `users-me` | `/users/me/**`          | any logged-in user       |
| `admin`    | `/users/**`             | ADMIN                    |
| `actuator` | `/actuator/health`      | anyone                   |

`X-API-Version` is `1.0` (the default) or `1.1`. Only `GET /api/expense` differs, so only **Get Expenses**
(1.1) and **Get Expenses v1.0** send the header.
