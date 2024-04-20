---
layout: post
title: JavaScript Package Development
category: blog
tags: [programming, javascript, typescript, npm, gitlab, tools]
published: false
---

I just (today!) finished setting up the main repo I use at work the way I wanted it. It accumulates tricks and tips from several different projects I've worked on in the past, trying to glean the best from everything. I feel I sense that I'll forget it all if I don't write it down, so here we are.

It's a bit GitLab-centric, since that's what I use at work. I'm confident it can be adapted to GitHub Actions or other CI/CD tools. Let me know if you write up such an adaptation—I'd love to link to it!

## Goals

Some things I need from the setup:

1. **Multiple packages**: I want to have multiple packages in the same repo. If you're developing a library you may not think you have multiple packages, but you probably do. You have the library itself, but you probably also have an example project or a CLI tool. You might have a website or a documentation generator. You might have a React component library with a Storybook. You might have a server and a client. You might have a server and a client and a shared library. You might have a server and a client and a shared library and a CLI tool. You get the idea.
1. **TypeScript**: I want to write TypeScript, and I want it to be checked and compiled. Browser-focused code uses `vite`, other things use `rollup` or `esbuild`. I'll talk more about those choices, and why you might use different ones in the same repo.
1. **Testing**: I want to run tests on every push. I use `vitest` for this, but the CI setup doesn't change if you use Jest or Mocha or whatever.
1. **Codegen and Formatting**: This project uses codegen extensively to generate client code in several languages. CI should check that the codegen is up-to-date. That is, running the codegen should produce no changes to the tracked files.
1. **Changesets**: I want to use [Changesets](https://github.com/changesets/changesets) to manage versioning and changelogs. New features and bugfixes should accumulate on the `main` branch, and we should be able to release them in a batch.
1. **Automatic Releases**: When we create a release, the packages should be automatically published to npm. But only the ones that changed!

## Tools

### pnpm

I use `pnpm` as an `npm` replacement, mostly for its workspace support. A "workspace" is a repo with several JS packages whose dependencies are all installed with `pnpm`. It also offers a way to run the same command across all projects in a workspace. I use this to run tests and build all packages at once, and even to publish only the packages that have changed.

### vite

`vite` is fast and low-config. It's great for browser-focused code. It's not great for libraries, because it doesn't output a CommonJS bundle.

### rollup

`rollup` is a bit slower than `vite`, but it's more flexible. It can output CommonJS, ES modules, UMD, and more. It's great for libraries. Rollup plugins are straightforward to write, so you can customize your build process easily. For example:

- I recently wrote a plugin to inline some `.sql` migration files into the JS bundle. The database code was part of a shared package, so the relative path to the migrations was different at the point where the migrations were run.
- Rahul wrote a plugin to extract `invariant(!isIframeWithNullOrigin, "Grammarly EditorSDK is not supported in IFrames with null origin")`, putting the string into a separate file, and replacing the code with something like `if (isIframeWithNullOrigin) throw new InvariantError(12)`. At runtime we showed a URL to the documentation for that error code. The bundle size was a bit smaller, but the really nice thing was that we could offer a page troubleshooting steps specific to each error and even update them after the code was released.

### esbuild

`esbuild` is the JS compiler that `vite` uses behind the scenes. It's not browser-specific and can output CommonJS. I've found that it's not as configurable as `rollup`, but it's very easy to set up. I use it for some oddball projects that are neither libraries nor browser-focused:

- A CLI tool that I distribute as a single node file. `esbuild --bundle --platform=node`.
- A couple of AWS Lambda functions that I also want to distribute as single files. Additionally, I want to omit the `@aws-sdk/*` dependencies, since the Lambda environment provides them and bundle size is important on lambdas. `esbuild --bundle --platform=node --target=node18 --external:@aws-sdk/client-s3 --external:@aws-sdk/client-secrets-manager`.

### changesets

`changesets` is a tool to keep track of version updates and write a changelog for a package. Every time you make a change to a package, you write a markdown file with a description of the change. The markdown file has some yaml frontmatter to specify the packages (many repos have multiple) and the type of change (major, minor, patch). Here's an example:

```
---
"@grammarly/avro-codegen": minor
"@grammarly/avro-parser": minor
---

Add support for @experiment() annotation
```

Every Merge Request that changes the behavior of a package also includes a file like that. The files, along with the changes, accumulate in the `.changeset` directory on the `main` branch. When you're ready to release, you run `changeset version` to eat all those files, turning them into CHANGELOG.md entries and increments to the versions of the affected packages.

We have two bits of CI automation around `changesets`:

1. A check that every MR that changes a package has a corresponding changeset file. If a change involves only tests, or otherwise doesn't need to be reflected in the changelog, you can create an empty changeset file.
2. A manual job to run `changeset version` and open an MR with the changes (that is, the removal of the .changeset files and updates to package.json and CHANGELOG.md files).

## gitlab-ci.yml

The gitlab-ci.yml file ends up quite long, so I'll break it up into sections with explanations for each piece.
