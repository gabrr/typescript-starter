# My Typescript Node Server Template

#### Adding typescript, types and tools as a dev dependency
```
yarn add typescript tsconfig-paths ts-node-dev ts-node @babel/preset-typescript @babel/preset-env @types/node -D
```

#### Tools as dependency
```
yarn add dotenv
```

**After installing TypeScript, tsc compiler command is avaiable.**

#### Generate TS Config file
```
npx tsc --init
```
OR
```
npx tsc --init --rootDir src --outDir build \
--esModuleInterop --resolveJsonModule --lib es6 \
--module commonjs --allowJs true --noImplicitAny true
```

**What are those commands?**

rootDir: This is where TypeScript looks for our code. We've configured it to look in the src/ folder. That's where we'll write our TypeScript.

outDir: Where TypeScript puts our compiled code. We want it to go to a build/ folder.

esModuleInterop: If you were in the JavaScript space over the past couple of years, you might have recognized that modules systems had gotten a little bit out of control (AMD, SystemJS, ES Modules, etc). For a topic that requires a much longer discussion, if we're using commonjs as our module system (for Node apps, you should be), then we need this to be set to true.

resolveJsonModule: If we use JSON in this project, this option allows TypeScript to use it.

lib: This option adds ambient types to our project, allowing us to rely on features from different Ecmascript versions, testing libraries, and even the browser DOM api. We'd like to utilize some es6 language features. This all gets compiled down to es5.

module: commonjs is the standard Node module system in 2019. Let's use that.

allowJs: If you're converting an old JavaScript project to TypeScript, this option will allow you to include .js files among .ts ones.

noImplicitAny: In TypeScript files, don't allow a type to be unexplicitly specified. Every type needs to either have a specific type or be explicitly declared any. No implicit anys.

**Editing the file generated — tsconfig.json —:**

Inside the `compilerOptions`, we want to add:
```
"paths": {
  "*": ["*"]
}
```
and at the index — out of `compilerOptions`, we want to add:
```
"exclude": ["node_modules", "build"],
"include": ["src"]
```

**Adding babel.config.js to root and insert it:**
```
module.exports = {
  presets: [
    ['@babel/preset-env', {targets: {node: 'current'}}],
    '@babel/preset-typescript',
  ],
};
```

**New scripts in package.json inside `scripts`:**

```
"build": "rimraf ./build && tsc",
"start": "yarn build && ts-node -r tsconfig-paths/register build/server.js",
"dev": "ts-node-dev --transpile-only -r tsconfig-paths/register src/server.ts",
```
**What are those commands?**
`rimraf` cleans the ./build folder and tsc transpiles the ts code to js.

`ts-node-dev` it is an alternative to nodemon, it will restart the server at every change.

`ts-node` with `tsconfig-paths/register` will guarantee that our absolute paths are working.

