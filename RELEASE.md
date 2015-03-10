Release
=======

Although not very tricky, it is quite easy to deploy something that
doesn't quite work as expected. The following steps navigate a through
some of the release gotchas and will hopefully result in a successful
release.

STEPS:
------

1. let other contributors know that you are preparing a release and to hold off on merging PRs or pushing new code
* ensure you using the latest node `v0.10.x` (but not v0.11.x for now,
   as it has a broken zlib which causes it to publish corrupted modules)
* run `./bin/changelog` and add output to `CHANGELOG.md`
* edit changelog output to be as user-friendly as possible (drop [INTERNAL] changes etc.)
* bump `package.json` version
* `rm -rf node_modules`
* `npm cache clear`
* `npm install`
* `npm pack`
* remove current installed version: `npm uninstall -g ember-cli`
* install the new package (for testing) `npm install -g ./ember-cli-<version>.tgz`
* `cd to/someplace/to/test/`
* ensure `ember version` is the newly packaged version
* ensure new project generation works  `ember new my-cool-test-project`
  this will fail with: `version not found: ember-cli@version`
* fixup deps: `cd my-cool-test-project`
* link your local ember-cli  `npm link ember-cli`
* install other deps: `npm install`
* install bower deps: `bower install`
* test the server: `ember s`
* test other things like generators
* generate an http-mock and test again
* test upgrades of other apps

If everything went well, release:

Please note, we have must have an extremely low tolerance for quirks
and failures we do not want our users to endure any extra pain.

1. go back to ember-cli directory
* `git add` the modified `package.json` and `CHANGELOG.md`
* Commit the changes `git commit -m "Release 0.x.x"` and push `git push`
* publish to npm
* `npm publish ./ember-cli-<version>.tgz`

Test published version

1. `npm uninstall -g ember-cli`
* `npm cache clear`
* `npm install -g ember-cli`
* ensure version is as expected `ember version`
* ensure new project generates
* ensure old project upgrades nicely

Tag the release
1. Under `Releases` on GitHub choose `Draft New Release`
* enter the new version number as the tag prefixed with `v` e.g. (`v0.1.12`)
* for release title choose a great name, no pressure
* in the description paste the upgrade instructions from the previous release, followed by the new CHANGELOG entry
* publish the release

Update the site

1. check out gh-pages
* update `_config.yml` version
* update `gh-pages/_posts/2012-01-01-changelog.md`