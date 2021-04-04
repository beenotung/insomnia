## npm install on develop branch

with fresh `~/.npm/_cacache`:

```
> time npm install
npm install  13.35s user 0.61s system 137% cpu 10.171 total
> time npm run clean
npm run clean  3.74s user 3.28s system 211% cpu 3.321 total
> time npm run bootstrap
npm run bootstrap  340.55s user 41.40s system 299% cpu 2:07.69 total
> du -sh .
2.7G	.
> du -sh --apparent-size .
2.0G	.
```

the second run:

```
> time npm install
npm install  13.56s user 0.68s system 130% cpu 10.930 total
> time npm run clean
npm run clean  10.40s user 9.06s system 354% cpu 5.485 total
> time npm run bootstrap
npm run bootstrap  316.56s user 39.37s system 298% cpu 1:59.26 total
```

npm cache:

```
> du -sh ~/.npm/_cacache
261M	/home/user/.npm/_cacache
> du -sh --apparent-size ~/.npm/_cacache
247M	/home/user/.npm/_cacache
```

**Remark**

When installing the packages, I saw some vulnerabilities packages, that could be discussed in separate issue.
