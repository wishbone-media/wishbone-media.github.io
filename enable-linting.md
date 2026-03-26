# Update NEXT GEN apps to use lint

## Actual commit example

This change largely consists of two commits: one to add the needed packages and add coding linting rules, one to fix the errors that failed the linting rules and reformat all `.js` and `.vue` files based on vue's "recommended" coding conventions.

For example you can see the commits on `mapit-web-next` here: [GitHub > mapit-web-next](https://github.com/wishbone-media/mapit-web-next/commits/master/):
- [Add packages & rules](https://github.com/wishbone-media/mapit-web-next/commit/88489d0a8a29a26e8feb8fd65b64b649bd4bda21)
- [Fix errors & reformat](https://github.com/wishbone-media/mapit-web-next/commit/3c2b918c11e828275bf0980a77b3bc6184746467)

We will walk through those two commits in steps below.

## Steps

### 1. Install packages

Run the following in your project root (e.g. buzz-web-next)

```
pnpm add -D lint-staged simple-git-hooks
pnpm approve-builds
```

(If `pnpm approve-builds` detected either simple-git-hooks or vue-demi need to be approved, approve them)

### 2. Manually edit your package.json

Please see the [Diff on package.json](https://github.com/wishbone-media/mapit-web-next/commit/88489d0a8a29a26e8feb8fd65b64b649bd4bda21#diff-7ae45ad102eab3b6d7e7896acd08c427a9b25b346470d7bc6507b6481575d519), the two packages in `devDependencies` are already added automatically by `pnpm add -D` we did in step 1. But you need to manually add/modify the rest, which are:
- scripts.prepare (new line)
- scripts.lint (modify)
- simple-git-hooks (new object)
- lint-staged (new object)

### 3. Replace your .prettierrc.json and eslint.config.js with the latest

Download those two files and replace them in your project root:
- [.prettierrc.json](https://github.com/wishbone-media/mapit-web-next/blob/master/.prettierrc.json)
- [eslint.config.js](https://github.com/wishbone-media/mapit-web-next/blob/master/eslint.config.js)

### 4. Run pnpm script to be sure

```
pnpm install
pnpm prepare
```

### 5. Check and commit those changes

You should have 5 files changed so far.

I've also done this on your `buzz-web-next` repo on a separated branch "linting": https://github.com/wishbone-media/buzz-web-next/commits/linting/

Right now your uncommited changes should be identical to this commit [build: better eslint/prettier setup via git-hook](https://github.com/wishbone-media/buzz-web-next/commit/1aec9df4831119000683b4b987c32ffe8a8b2219)

I named this commit "build: better eslint/prettier setup via git-hook".

Once you are happy that all 5 files are changed correctly, commit your change. If you somehow have trouble, just checkout the "linting" branch from `buzz-web-next`, it contains 1 commit ahead of the master branch with my commit with those 5 changed files.

### 6. Lint time

Now run `pnpm lint`, the linting scripts will check all your existing `.js` and `.vue` files, automatically reformat for basic styling. It will throw errors for where your code fails vue's recommended rules.

Your buzz-web-next will fail here, I think most if not all errors are due to failing no-unused-vars rules. They should be relatively easy to fix, but if you got any issues please let me know I will sort them out for you. Go ahead and fix them until `pnpm lint` fully pass without error.

I name this commit to "style: reformat via lint from updated eslint/prettier config, with adjustments"; you can now commit yours and finally, push those 2 commits to Github.

In the future when you commit, the git commit will fail if the changes in your commit fail vue's recommended rules. This is a guardrail to keep your code quality better.

### 7. One more thing

Now we have linting and formatting automatically at git commit, it makes your life easier if you make those two changes to all your BOLT NEXT GEN apps (e.g. mapit-web-next, buzz-web-next, etc.) config in PHPSTORM:

- Enable PHPStorm auto eslint: ![PHPStorm ESLint Config](https://assets.letsbolt.com.au/dev/ESLint.png)
- Enable PHPStorm auto prettier: ![PHPStorm Prettier Config](https://assets.letsbolt.com.au/dev/Prettier.png)
