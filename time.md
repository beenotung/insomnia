| installer | bootstrap time | repo size | packages cache size |
|---|---|---|---|
| npm | 316s | 2.7G | 261M |
| pnpm | 160s | 1.5G | 1020M |

## npm install on develop branch

with fresh `~/.npm/_cacache`:

```bash
> time npm install
npm install  13.35s user 0.61s system 137% cpu 10.171 total
> time npm run clean
npm run clean  3.74s user 3.28s system 211% cpu 3.321 total
> time npm run bootstrap
npm run bootstrap  340.55s user 41.40s system 299% cpu 2:07.69 total
```

the second run:

```bash
> time npm install
npm install  13.56s user 0.68s system 130% cpu 10.930 total
> time npm run clean
npm run clean  10.40s user 9.06s system 354% cpu 5.485 total
> time npm run bootstrap
npm run bootstrap  316.56s user 39.37s system 298% cpu 1:59.26 total
```

repo size:

```bash
> du -sh .
2.7G	.
> du -sh --apparent-size .
2.0G	.
```

cache size:

```bash
> du -sh ~/.npm/_cacache
261M	/home/user/.npm/_cacache
> du -sh --apparent-size ~/.npm/_cacache
247M	/home/user/.npm/_cacache
```


## pnpm install on pnpm branch

with fresh `~/.npm/_cacache` and `~/.pnpm-store`:

```bash
> time pnpm install
pnpm install  7.55s user 1.13s system 154% cpu 5.615 total
> time npm run clean
npm run clean  2.18s user 1.09s system 165% cpu 1.979 total
> time npm run boostrap
npm run bootstrap  212.83s user 27.95s system 289% cpu 1:23.18 total
```

the second run:

```bash
> time pnpm install
pnpm install  2.23s user 0.21s system 134% cpu 1.815 total
> time npm run clean
npm run clean  2.29s user 0.91s system 159% cpu 2.009 total
> time npm run boostrap
npm run bootstrap  163.08s user 19.68s system 276% cpu 1:05.99 total
```

repo size:

```bash
> du -sh .
1.5G	.
> du -sh --apparent-size .
1.3G	.
```

cache size:

```bash
> du -sh ~/.npm/_cacache
504K	/home/user/.npm/_cacache
> du -sh --apparent-size ~/.npm/_cacache
485K	/home/user/.npm/_cacache
> du -sh ~/.pnpm-store/
1020M	/home/user/.pnpm-store/
> du -sh --apparent-size ~/.pnpm-store/
834M	/home/beenotung/.pnpm-store/
```

## Remarks

1. When installing the packages, I saw some vulnerabilities packages, that could be discussed in separate issue.
2. The pnpm run is not fully optimized, I saw lerna installing external dependencies with npm when running the `bootstrap` script.
3. Before each run, I reset repo using below commands, which will delete all `node_modules` and built files.
4. pnpm can install and build the packages in monorepo recursively and parallelly using `pnpm install -r` but I'm not using it as comparsion because I'm not sure if `lerna bootstrap` is running additional workflow.

```bash
git add . -A
git reset --hard HEAD
git status --ignored | tail -n +6 | head -n -2 | xargs rm -rf
```
