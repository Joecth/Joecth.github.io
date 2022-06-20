---
layout: post
categories: NodeJS
tag: [] 
date: 2021-06-17
---





```shell
$ nvm use v16.13.2
Now using node v16.13.2 (npm v8.1.2)
(base)
# joe @ J-M1-Pro-16 in ~ [16:23:32]
$ nvm ls
       v12.18.0
->     v16.13.2
         system
default -> v12.18.0
node -> stable (-> v16.13.2) (default)
stable -> 16.13 (-> v16.13.2) (default)
iojs -> N/A (default)
unstable -> N/A (default)
lts/* -> lts/gallium (-> N/A)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.12 (-> N/A)
lts/fermium -> v14.19.3 (-> N/A)
lts/gallium -> v16.15.1 (-> N/A)
(base)

# joe @ J-M1-Pro-16 in ~ [16:28:44]
$ which npm
/Users/joe/.nvm/versions/node/v16.13.2/bin/npm
(base)
# joe @ J-M1-Pro-16 in ~ [16:28:48]
$ which node
/Users/joe/.nvm/versions/node/v16.13.2/bin/node
(base)
# joe @ J-M1-Pro-16 in ~ [16:29:07]
```



```shell
# joe @ J-M1-Pro-16 in ~/side_projects/sallyer-projects/serverless on git:develop x [16:50:45] C:130
$ mv .env .env.local

# joe @ J-M1-Pro-16 in ~/side_projects/sallyer-projects/serverless on git:develop x [16:50:54]
$ npm run dev

> sallyer-next@0.0.0 dev
> next dev -p 3005

ready - started server on 0.0.0.0:3005, url: http://localhost:3005
info  - Loaded env from /Users/joe/side_projects/sallyer-projects/serverless/.env.local
event - compiled client and server successfully in 1018 ms (1148 modules)
wait  - compiling / (client and server)...
```