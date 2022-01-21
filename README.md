# Parse Platform Backend Starter Template

## Installation
```sh
cp .env.example .env
# sets the master key
sed -i "s/CHANGE_ME_KEY/$(cat /dev/urandom | tr -dc '[:alpha:]' | fold -w ${1:-48} | head -n 1)/g" .env
# sets the mongodb password
sed -i "s/randompassword/$(cat /dev/urandom | tr -dc '[:alpha:]' | fold -w ${1:-20} | head -n 1)/g" .env

# now change the contents of .env, then continue

cp config.json.example config.json
while IFS='=' read -r name value ; do sed -i "s~\$${name}~${value}~g" config.json; done < <(cat .env)
```

## Reload on changes
After changes are made to **cloud code**, the server must be restarted. This can be done by the following command `docker-compose restart parse`.

TODO: Hot reload

For changes on the **live query**, edit the `PARSE_SERVER_LIVE_QUERY` variable and then run `docker-compose up -d` again.

Changes on the dashboard (`config.json`) will be applied by restarting the dashboard with `docker-compose restart parse-dashboard`.

## Dashboard

> ❗Please change or remove these!

| username | password |
|----------|----------|
| user     | secure   |
| admin    | admin    |

### Adding a user
```sh
docker-compose exec parse-dashboard node /src/Parse-Dashboard/index.js --createUser
```
Add the user in `config.json` under `$.users[]`.

If `useEncryptedPasswords` is set to `true`, you have to use bcrypt-encrypted password.

Reload the dashboard, see above.

### Adding MFA
```sh
docker-compose exec parse-dashboard node /src/Parse-Dashboard/index.js --createMFA
```

Common settings are `SHA1` with `6` digits and `30` seconds validity. Similar to adding a new user, the output has to be added to `config.json` aswell.

Reload the dashboard, see above.
