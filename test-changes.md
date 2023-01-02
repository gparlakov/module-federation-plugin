# Allow v12 of mf to use v15 of runtime

## Why?

When host app is v12 and the remote app is v14 the module federation via @angular/elements does not work. That's b/c v14 needs to use `import` statements in the `remoteEntry.js` file so the host needs to import that file as a JavaScript module - which necessitates the use of the v15 runtime package.

## Did you test it?
When copy-pasting the code for the dynamic-federation.js (from v15) and WebComponentWrapperComponent from v12 locally it works like a charm with the `type:'module'` remote application.

##  Testing the change with verdaccio (local npm repository)

Goal: Ensure that v15 of runtime is installed when isntalling v12 of the main package

- checkout branch
- have a docker instance running (like docker-desktop)
- `docker-compose -f .\.verdaccio\docker-compose.yml up`
- `npm ci`
- `npm run build`
- `npm publish dist/libs/mf --registry http://localhost:4873` - only main package
- In playground

- `cd apps\playground`
- `npm init -y` (so we can install @angular-architects/module-federation packages in here)
- `npm i @angular-architects/module-federation --registry http://localhost:4873 @angular-architects/module-federation-runtime@15`
- verify that there's no `node_modules/@angular-architects/module-federation/node_modules/@angular-architects/module-federation-runtime` at v12 which means that the installed module-federation-runtime v15 will be used

- ```ps
  cat .\node_modules\@angular-architects\module-federation\package.json | findstr ver
  "version": "12.5.0"
  cat .\node_modules\@angular-architects\module-federation-runtime\package.json | findstr ver
  "version": "15.0.2"
  ```


