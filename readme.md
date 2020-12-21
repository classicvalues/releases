# electron-releases

![Test](https://github.com/electron/releases/workflows/Test/badge.svg)

> Complete and up-to-date info about every release of Electron.

This package:

- includes all [GitHub Releases](https://developer.github.com/v3/repos/releases/#get-a-single-release) data about Electron.
- does not include draft releases.
- includes prereleases which are not published to npm.
- tracks which versions are published to npm.
- tracks [npm dist-tags](https://docs.npmjs.com/cli/dist-tag) like `latest` and `beta`.
- includes V8, Chromium, and Node.js version data.
- includes [GitHub-flavored HTML](https://ghub.io/hubdown) for each release's changelog.
- ignores npm versions from the days before [Electron was `electron`](https://electronjs.org/blog/npm-install-electron).
- is [updated regularly](#updates).

## Sources

This module collects metadata from various sources:

- [GitHub Releases of `electron/electron`](https://github.com/electron/electron)
- [dist-tags from npm registry for `electron`](https://registry.npmjs.com/electron)
- [dist-tags from npm registry `electron-prebuilt`](https://registry.npmjs.com/electron-prebuilt)
- [Dependency data for Chromium, Node.js, V8, etc](https://atom.io/download/electron/index.json)

## Installation

```sh
npm i electron-releases
```

## Usage

The module exports an array of release objects:

```js
const releases = require('electron-releases')

// find newest version:
releases[0].tag_name // => 'v1.8.2-beta.3'

// find `latest` on npm, which is not necessarily the most recent release:
releases.find(release => release.npm_dist_tag === 'latest')

// find `beta` on npm:
releases.find(release => release.npm_dist_tag === 'beta')
```

### Lite Version

The default export is about 75MB, as it includes a lot of metadata from the
GitHub API like release assets.

If you just need the basic info like version numbers, npm dist tags, and publish dates, there's a much smaller (<500K) dataset you can use:

```js
require('electron-releases/lite.json')
```

You can also get this at [unpkg.com/electron-releases/lite.json](https://unpkg.com/electron-releases/lite.json)

### Data

Each release contains all the data returned by the
[GitHub Releases API](https://docs.github.com/en/free-pro-team@latest/rest/reference/repos#releases),
plus some extra properties:

- `version` (String) - the same thing as `dist_tag`, but without the `v` for convenient [semver comparisons](https://github.com/npm/node-semver#usage).
- `npm_dist_tags` (Array<String>) - an array of [npm dist-tags](https://docs.npmjs.com/cli/dist-tag) like `"latest"` or `"beta"`. Most releases will have an empty array for this property.
- `npm_package_name` (String) - For packages published to npm, this will be `electron` or `electron-prebuilt`. For packages not published to npm, this property will not exist.
- `total_downloads` (Number) - Total downloads of all assets in the release that
  have a [detectable platform](https://github.com/zeke/platform-utils#api) in their
  filename like `.zip`, `.dmg`, `.exe`, `.rpm`, `.deb`, etc.
- `deps` (Object) - version numbers for Electron dependencies.
  - `v8` (String)
  - `chromium` (String)
  - `node` (String)
  - etc..

## Updates

This module is self-publishing. It runs in a GitHub Action cron job every
six hours. A new version of this module is published if any of
the following change:

- number of Electron releases on GitHub
- number of Electron releases on npm
- npm `electron@beta` version
- npm `electron@latest` version

If none of these has changed, the build process aborts and runs again ten minutes
later. For more detail, see [script/release.sh](script/release.sh)

The Heroku app is also synced to the GitHub repo, so every push to the
`master` branch will automatically deploy a new version of this app.

### Manually update

If you need to modify any file in the `script` folder, you'll also want
to manually regenerate this module's output files. You can do so with
the following steps:

1. Create a [personal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/#creating-a-token). Note that if Electron maintainers must enable SSO
for the `electron` org for this token to work.
1. Copy the `.env.example` file into a separate `.env` file.
    ```sh
    cp .env.example .env
    ```
1. Paste your token into the `GH_TOKEN` field of the `.env` file.
1. Build the module.
    ```sh
    npm run build
    ```
1. Check if all tests passed:
    ```sh
    npm test
    ```

## Tests

```sh
npm install
npm test
```

## License

MIT

## Releases

<!-- START RELEASES TABLE -->
|Tag|Published|npm|Prerelease|Module Version|Node|Chrome|Downloads|
|----|----|----|----|----|----|----|----|
|[v13.0.0-nightly.20201221](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201221)|2020-12-21|nightly|yes|89|14.15.2|89.0.4349.0|73|
|[v13.0.0-nightly.20201216](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201216)|2020-12-16||yes|89|14.15.1|89.0.4349.0|221|
|[v13.0.0-nightly.20201215](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201215)|2020-12-15||yes|89|14.15.1|89.0.4349.0|96|
|[v13.0.0-nightly.20201214](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201214)|2020-12-14||yes|89|14.15.1|89.0.4328.0|91|
|[v13.0.0-nightly.20201211](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201211)|2020-12-11||yes|89|14.15.1|89.0.4328.0|162|
|[v13.0.0-nightly.20201210](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201210)|2020-12-10||yes|89|14.15.1|89.0.4328.0|100|
|[v13.0.0-nightly.20201209](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201209)|2020-12-09||yes|89|14.15.1|89.0.4328.0|109|
|[v13.0.0-nightly.20201208](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201208)|2020-12-08||yes|89|14.15.1|89.0.4328.0|104|
|[v13.0.0-nightly.20201207](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201207)|2020-12-07||yes|89|14.15.1|89.0.4328.0|91|
|[v13.0.0-nightly.20201204](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201204)|2020-12-04||yes|89|14.15.1|89.0.4328.0|128|
|[v13.0.0-nightly.20201203](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201203)|2020-12-03||yes|89|14.15.1|89.0.4328.0|170|
|[v13.0.0-nightly.20201202](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201202)|2020-12-02||yes|89|14.15.1|89.0.4328.0|174|
|[v13.0.0-nightly.20201201](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201201)|2020-12-01||yes|89|14.15.1|89.0.4328.0|180|
|[v13.0.0-nightly.20201130](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201130)|2020-11-30||yes|89|14.15.1|89.0.4328.0|308|
|[v13.0.0-nightly.20201127](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201127)|2020-11-27||yes|89|14.15.1|89.0.4328.0|371|
|[v13.0.0-nightly.20201126](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201126)|2020-11-26||yes|89|14.15.1|89.0.4328.0|308|
|[v13.0.0-nightly.20201124](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201124)|2020-11-24||yes|89|14.15.1|89.0.4328.0|336|
|[v13.0.0-nightly.20201123](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201123)|2020-11-23||yes|87|14.15.1|89.0.4328.0|308|
|[v13.0.0-nightly.20201119](https://github.com/electron/electron/releases/tag/v13.0.0-nightly.20201119)|2020-11-19||yes|87|14.15.1|89.0.4328.0|386|
|[v12.0.0-nightly.20201116](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20201116)|2020-11-17||yes|87|14.15.0|88.0.4324.0|460|
|[v12.0.0-nightly.20201112](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20201112)|2020-11-12||yes|87|14.15.0|88.0.4306.0|479|
|[v12.0.0-nightly.20201111](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20201111)|2020-11-11||yes|87|14.15.0|88.0.4306.0|318|
|[v12.0.0-nightly.20201106](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20201106)|2020-11-06||yes|87|14.15.0|88.0.4306.0|443|
|[v12.0.0-nightly.20201105](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20201105)|2020-11-05||yes|87|14.15.0|88.0.4306.0|308|
|[v12.0.0-nightly.20201104](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20201104)|2020-11-04||yes|87|14.15.0|88.0.4306.0|323|
|[v12.0.0-nightly.20201103](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20201103)|2020-11-03||yes|87|14.15.0|88.0.4306.0|312|
|[v12.0.0-nightly.20201102](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20201102)|2020-11-02||yes|87|14.15.0|88.0.4306.0|327|
|[v12.0.0-nightly.20201030](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20201030)|2020-10-30||yes|87|14.15.0|88.0.4306.0|289|
|[v12.0.0-nightly.20201026](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20201026)|2020-10-26||yes|87|14.14.0|88.0.4292.0|499|
|[v12.0.0-nightly.20201023](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20201023)|2020-10-23||yes|87|14.14.0|88.0.4292.0|369|
|[v12.0.0-nightly.20201015](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20201015)|2020-10-15||yes|87|14.13.1|87.0.4268.0|511|
|[v12.0.0-nightly.20201014](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20201014)|2020-10-14||yes|87|14.13.1|87.0.4268.0|325|
|[v12.0.0-nightly.20201013](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20201013)|2020-10-13||yes|87|14.13.1|87.0.4268.0|331|
|[v12.0.0-nightly.20200914](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20200914)|2020-09-14||yes|82|12.18.3|86.0.4234.0|1095|
|[v12.0.0-nightly.20200911](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20200911)|2020-09-11||yes|82|12.18.3|86.0.4234.0|231|
|[v12.0.0-nightly.20200910](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20200910)|2020-09-10||yes|82|12.18.3|86.0.4234.0|347|
|[v12.0.0-nightly.20200907](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20200907)|2020-09-07||yes|82|12.18.3|86.0.4234.0|238|
|[v12.0.0-nightly.20200903](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20200903)|2020-09-03||yes|82|12.18.3|86.0.4234.0|476|
|[v12.0.0-nightly.20200902](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20200902)|2020-09-02||yes|82|12.18.3|86.0.4234.0|249|
|[v12.0.0-nightly.20200831](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20200831)|2020-08-31||yes|82|12.18.3|86.0.4234.0|279|
|[v12.0.0-nightly.20200827](https://github.com/electron/electron/releases/tag/v12.0.0-nightly.20200827)|2020-08-27||yes|82|12.18.3|86.0.4234.0|321|
|[v12.0.0-beta.12](https://github.com/electron/electron/releases/tag/v12.0.0-beta.12)|2020-12-21|beta, beta-12-x-y|yes|87|14.15.1|89.0.4328.0|257|
|[v12.0.0-beta.11](https://github.com/electron/electron/releases/tag/v12.0.0-beta.11)|2020-12-17||yes|87|14.15.1|89.0.4328.0|982|
|[v12.0.0-beta.10](https://github.com/electron/electron/releases/tag/v12.0.0-beta.10)|2020-12-14||yes|87|14.15.1|89.0.4328.0|1066|
|[v12.0.0-beta.9](https://github.com/electron/electron/releases/tag/v12.0.0-beta.9)|2020-12-11||yes|87|14.15.1|89.0.4328.0|928|
|[v12.0.0-beta.8](https://github.com/electron/electron/releases/tag/v12.0.0-beta.8)|2020-12-10||yes|87|14.15.1|89.0.4328.0|804|
|[v12.0.0-beta.7](https://github.com/electron/electron/releases/tag/v12.0.0-beta.7)|2020-12-07||yes|87|14.15.1|89.0.4328.0|1016|
|[v12.0.0-beta.6](https://github.com/electron/electron/releases/tag/v12.0.0-beta.6)|2020-12-03||yes|87|14.15.1|89.0.4328.0|1110|
|[v12.0.0-beta.5](https://github.com/electron/electron/releases/tag/v12.0.0-beta.5)|2020-11-30||yes|87|14.15.1|89.0.4328.0|1440|
|[v12.0.0-beta.4](https://github.com/electron/electron/releases/tag/v12.0.0-beta.4)|2020-11-26||yes|87|14.15.1|89.0.4328.0|1497|
|[v12.0.0-beta.3](https://github.com/electron/electron/releases/tag/v12.0.0-beta.3)|2020-11-23||yes|87|14.15.1|89.0.4328.0|1287|
|[v12.0.0-beta.1](https://github.com/electron/electron/releases/tag/v12.0.0-beta.1)|2020-11-19||yes|87|14.15.1|89.0.4328.0|1357|
|[v11.1.0](https://github.com/electron/electron/releases/tag/v11.1.0)|2020-12-12|latest, 11-x-y|no|85|12.18.3|87.0.4280.88|52408|
|[v11.0.5](https://github.com/electron/electron/releases/tag/v11.0.5)|2020-12-11||no|85|12.18.3|87.0.4280.88|40295|
|[v11.0.4](https://github.com/electron/electron/releases/tag/v11.0.4)|2020-12-07||no|85|12.18.3|87.0.4280.67|28566|
|[v11.0.3](https://github.com/electron/electron/releases/tag/v11.0.3)|2020-11-24||no|85|12.18.3|87.0.4280.67|130275|
|[v11.0.2](https://github.com/electron/electron/releases/tag/v11.0.2)|2020-11-19||no|85|12.18.3|87.0.4280.67|41575|
|[v11.0.1](https://github.com/electron/electron/releases/tag/v11.0.1)|2020-11-17||no|85|12.18.3|87.0.4280.60|20216|
|[v11.0.0](https://github.com/electron/electron/releases/tag/v11.0.0)|2020-11-16||no|85|12.18.3|87.0.4280.60|22279|
|[v11.0.0-nightly.20200826](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200826)|2020-08-26||yes|82|12.18.3|86.0.4234.0|373|
|[v11.0.0-nightly.20200825](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200825)|2020-08-25||yes|82|12.18.3|86.0.4234.0|364|
|[v11.0.0-nightly.20200824](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200824)|2020-08-24||yes|82|12.18.3|86.0.4234.0|321|
|[v11.0.0-nightly.20200822](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200822)|2020-08-23||yes|82|12.18.3|86.0.4234.0|227|
|[v11.0.0-nightly.20200812](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200812)|2020-08-12||yes|82|12.18.3|86.0.4209.0|4683|
|[v11.0.0-nightly.20200811](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200811)|2020-08-11||yes|82|12.18.3|86.0.4209.0|339|
|[v11.0.0-nightly.20200805](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200805)|2020-08-05||yes|82|12.18.3|86.0.4209.0|503|
|[v11.0.0-nightly.20200804](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200804)|2020-08-04||yes|82|12.18.3|86.0.4209.0|261|
|[v11.0.0-nightly.20200803](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200803)|2020-08-03||yes|82|12.18.3|86.0.4209.0|1514|
|[v11.0.0-nightly.20200731](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200731)|2020-07-31||yes|82|12.18.3|86.0.4209.0|328|
|[v11.0.0-nightly.20200730](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200730)|2020-07-30||yes|82|12.18.3|86.0.4209.0|256|
|[v11.0.0-nightly.20200729](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200729)|2020-07-29||yes|82|12.18.3|86.0.4209.0|299|
|[v11.0.0-nightly.20200724](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200724)|2020-07-24||yes|82|12.18.2|86.0.4209.0|370|
|[v11.0.0-nightly.20200723](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200723)|2020-07-23||yes|82|12.18.2|86.0.4209.0|264|
|[v11.0.0-nightly.20200721](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200721)|2020-07-22||yes|82|12.18.2|86.0.4203.0|706|
|[v11.0.0-nightly.20200720](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200720)|2020-07-20||yes|82|12.18.2|86.0.4203.0|260|
|[v11.0.0-nightly.20200717](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200717)|2020-07-17||yes|82|12.18.2|86.0.4203.0|241|
|[v11.0.0-nightly.20200716](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200716)|2020-07-16||yes|82|12.18.2|86.0.4203.0|227|
|[v11.0.0-nightly.20200709](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200709)|2020-07-09||yes|82|12.18.2|85.0.4179.0|251|
|[v11.0.0-nightly.20200708](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200708)|2020-07-08||yes|82|12.18.2|85.0.4179.0|424|
|[v11.0.0-nightly.20200707](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200707)|2020-07-07||yes|82|12.18.2|85.0.4179.0|222|
|[v11.0.0-nightly.20200706](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200706)|2020-07-06||yes|82|12.18.2|85.0.4179.0|223|
|[v11.0.0-nightly.20200703](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200703)|2020-07-03||yes|82|12.18.2|85.0.4179.0|292|
|[v11.0.0-nightly.20200702](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200702)|2020-07-02||yes|82|12.18.2|85.0.4179.0|229|
|[v11.0.0-nightly.20200701](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200701)|2020-07-01||yes|82|12.18.1|85.0.4179.0|228|
|[v11.0.0-nightly.20200619](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200619)|2020-06-19||yes|82|12.18.1|85.0.4162.0|487|
|[v11.0.0-nightly.20200618](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200618)|2020-06-18||yes|82|12.18.0|85.0.4162.0|213|
|[v11.0.0-nightly.20200617](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200617)|2020-06-17||yes|82|12.16.3|85.0.4162.0|214|
|[v11.0.0-nightly.20200616](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200616)|2020-06-16||yes|82|12.16.3|85.0.4162.0|215|
|[v11.0.0-nightly.20200615](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200615)|2020-06-15||yes|82|12.16.3|85.0.4162.0|152|
|[v11.0.0-nightly.20200611](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200611)|2020-06-11||yes|82|12.16.3|85.0.4162.0|304|
|[v11.0.0-nightly.20200610](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200610)|2020-06-10||yes|82|12.16.3|85.0.4162.0|155|
|[v11.0.0-nightly.20200609](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200609)|2020-06-09||yes|82|12.16.3|85.0.4162.0|211|
|[v11.0.0-nightly.20200604](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200604)|2020-06-04||yes|82|12.16.3|85.0.4162.0|141|
|[v11.0.0-nightly.20200603](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200603)|2020-06-03||yes|82|12.16.3|85.0.4162.0|359|
|[v11.0.0-nightly.20200602](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200602)|2020-06-02||yes|82|12.16.3|85.0.4162.0|210|
|[v11.0.0-nightly.20200529](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200529)|2020-05-29||yes|82|12.16.3|85.0.4156.0|287|
|[v11.0.0-nightly.20200526](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200526)|2020-05-26||yes|82|12.16.3|84.0.4129.0|285|
|[v11.0.0-nightly.20200525](https://github.com/electron/electron/releases/tag/v11.0.0-nightly.20200525)|2020-05-25||yes|82|12.16.3|84.0.4129.0|186|
|[v11.0.0-beta.23](https://github.com/electron/electron/releases/tag/v11.0.0-beta.23)|2020-11-14|beta-11-x-y|yes|85|12.18.3|87.0.4280.47|1881|
|[v11.0.0-beta.22](https://github.com/electron/electron/releases/tag/v11.0.0-beta.22)|2020-11-12||yes|85|12.18.3|87.0.4280.47|1385|
|[v11.0.0-beta.20](https://github.com/electron/electron/releases/tag/v11.0.0-beta.20)|2020-11-06||yes|85|12.18.3|87.0.4280.40|7221|
|[v11.0.0-beta.19](https://github.com/electron/electron/releases/tag/v11.0.0-beta.19)|2020-11-02||yes|85|12.18.3|87.0.4280.27|3100|
|[v11.0.0-beta.18](https://github.com/electron/electron/releases/tag/v11.0.0-beta.18)|2020-10-29||yes|85|12.18.3|87.0.4280.27|2013|
|[v11.0.0-beta.17](https://github.com/electron/electron/releases/tag/v11.0.0-beta.17)|2020-10-26||yes|85|12.18.3|87.0.4280.27|4878|
|[v11.0.0-beta.16](https://github.com/electron/electron/releases/tag/v11.0.0-beta.16)|2020-10-24||yes|85|12.18.3|87.0.4280.27|2168|
|[v11.0.0-beta.13](https://github.com/electron/electron/releases/tag/v11.0.0-beta.13)|2020-10-15||yes|85|12.18.3|87.0.4280.11|3121|
|[v11.0.0-beta.12](https://github.com/electron/electron/releases/tag/v11.0.0-beta.12)|2020-10-14||yes|85|12.18.3|87.0.4280.11|4219|
|[v11.0.0-beta.11](https://github.com/electron/electron/releases/tag/v11.0.0-beta.11)|2020-10-05||yes|85|12.18.3|87.0.4251.1|2153|
|[v11.0.0-beta.9](https://github.com/electron/electron/releases/tag/v11.0.0-beta.9)|2020-09-28||yes|82|12.18.3|87.0.4251.1|4295|
|[v11.0.0-beta.8](https://github.com/electron/electron/releases/tag/v11.0.0-beta.8)|2020-09-25||yes|82|12.18.3|87.0.4251.1|1626|
|[v11.0.0-beta.7](https://github.com/electron/electron/releases/tag/v11.0.0-beta.7)|2020-09-14||yes|82|12.18.3|86.0.4234.0|2528|
|[v11.0.0-beta.6](https://github.com/electron/electron/releases/tag/v11.0.0-beta.6)|2020-09-10||yes|82|12.18.3|86.0.4234.0|2430|
|[v11.0.0-beta.5](https://github.com/electron/electron/releases/tag/v11.0.0-beta.5)|2020-09-07||yes|82|12.18.3|86.0.4234.0|1633|
|[v11.0.0-beta.4](https://github.com/electron/electron/releases/tag/v11.0.0-beta.4)|2020-09-03||yes|82|12.18.3|86.0.4234.0|4840|
|[v11.0.0-beta.3](https://github.com/electron/electron/releases/tag/v11.0.0-beta.3)|2020-08-31||yes|82|12.18.3|86.0.4234.0|2023|
|[v11.0.0-beta.1](https://github.com/electron/electron/releases/tag/v11.0.0-beta.1)|2020-08-26||yes|82|12.18.3|86.0.4234.0|2391|
|[v10.2.0](https://github.com/electron/electron/releases/tag/v10.2.0)|2020-12-12|10-x-y|no|82|12.16.3|85.0.4183.121|9258|
|[v10.1.7](https://github.com/electron/electron/releases/tag/v10.1.7)|2020-12-08||no|82|12.16.3|85.0.4183.121|8628|
|[v10.1.6](https://github.com/electron/electron/releases/tag/v10.1.6)|2020-11-18||no|82|12.16.3|85.0.4183.121|45697|
|[v10.1.5](https://github.com/electron/electron/releases/tag/v10.1.5)|2020-10-23||no|82|12.16.3|85.0.4183.121|176386|
|[v10.1.4](https://github.com/electron/electron/releases/tag/v10.1.4)|2020-10-20||no|82|12.16.3|85.0.4183.121|73666|
|[v10.1.3](https://github.com/electron/electron/releases/tag/v10.1.3)|2020-09-29||no|82|12.16.3|85.0.4183.121|165328|
|[v10.1.2](https://github.com/electron/electron/releases/tag/v10.1.2)|2020-09-14||no|82|12.16.3|85.0.4183.98|114762|
|[v10.1.1](https://github.com/electron/electron/releases/tag/v10.1.1)|2020-09-01||no|82|12.16.3|85.0.4183.93|128572|
|[v10.1.0](https://github.com/electron/electron/releases/tag/v10.1.0)|2020-08-28||no|82|12.16.3|85.0.4183.87|53929|
|[v10.0.1](https://github.com/electron/electron/releases/tag/v10.0.1)|2020-08-27||no|82|12.16.3|85.0.4183.86|3664|
|[v10.0.0](https://github.com/electron/electron/releases/tag/v10.0.0)|2020-08-24||no|82|12.16.3|85.0.4183.84|92687|
|[v10.0.0-nightly.20200521](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200521)|2020-05-21||yes|82|12.16.3|84.0.4129.0|335|
|[v10.0.0-nightly.20200520](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200520)|2020-05-20||yes|82|12.16.3|84.0.4129.0|197|
|[v10.0.0-nightly.20200519](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200519)|2020-05-19||yes|82|12.16.3|84.0.4129.0|180|
|[v10.0.0-nightly.20200518](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200518)|2020-05-18||yes|82|12.16.3|84.0.4129.0|189|
|[v10.0.0-nightly.20200515](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200515)|2020-05-15||yes|82|12.16.3|84.0.4129.0|145|
|[v10.0.0-nightly.20200514](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200514)|2020-05-14||yes|82|12.16.3|84.0.4129.0|297|
|[v10.0.0-nightly.20200513](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200513)|2020-05-13||yes|82|12.16.3|84.0.4129.0|175|
|[v10.0.0-nightly.20200512](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200512)|2020-05-12||yes|82|12.16.3|84.0.4129.0|180|
|[v10.0.0-nightly.20200511](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200511)|2020-05-11||yes|82|12.16.3|84.0.4129.0|203|
|[v10.0.0-nightly.20200508](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200508)|2020-05-08||yes|82|12.16.3|84.0.4129.0|423|
|[v10.0.0-nightly.20200507](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200507)|2020-05-07||yes|82|12.16.3|84.0.4129.0|218|
|[v10.0.0-nightly.20200506](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200506)|2020-05-06||yes|82|12.16.3|84.0.4129.0|170|
|[v10.0.0-nightly.20200505](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200505)|2020-05-05||yes|82|12.16.3|84.0.4129.0|173|
|[v10.0.0-nightly.20200504](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200504)|2020-05-04||yes|82|12.16.3|84.0.4129.0|130|
|[v10.0.0-nightly.20200501](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200501)|2020-05-01||yes|82|12.16.3|84.0.4129.0|273|
|[v10.0.0-nightly.20200430](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200430)|2020-04-30||yes|82|12.16.3|84.0.4125.0|158|
|[v10.0.0-nightly.20200429](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200429)|2020-04-29||yes|82|12.16.2|84.0.4125.0|156|
|[v10.0.0-nightly.20200428](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200428)|2020-04-28||yes|82|12.16.2|84.0.4125.0|183|
|[v10.0.0-nightly.20200427](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200427)|2020-04-27||yes|82|12.16.2|84.0.4125.0|160|
|[v10.0.0-nightly.20200423](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200423)|2020-04-24||yes|82|12.16.2|84.0.4121.0|206|
|[v10.0.0-nightly.20200422](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200422)|2020-04-22||yes|82|12.16.2|84.0.4121.0|298|
|[v10.0.0-nightly.20200417](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200417)|2020-04-17||yes|82|12.16.2|84.0.4115.0|318|
|[v10.0.0-nightly.20200416](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200416)|2020-04-16||yes|82|12.16.2|84.0.4115.0|297|
|[v10.0.0-nightly.20200415](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200415)|2020-04-15||yes|82|12.16.2|84.0.4115.0|176|
|[v10.0.0-nightly.20200414](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200414)|2020-04-14||yes|82|12.16.1|84.0.4114.0|166|
|[v10.0.0-nightly.20200413](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200413)|2020-04-13||yes|82|12.16.1|83.0.4095.0|167|
|[v10.0.0-nightly.20200410](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200410)|2020-04-10||yes|82|12.16.1|83.0.4095.0|144|
|[v10.0.0-nightly.20200408](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200408)|2020-04-08||yes|82|12.16.1|83.0.4095.0|284|
|[v10.0.0-nightly.20200406](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200406)|2020-04-06||yes|82|12.16.1|83.0.4087.0|243|
|[v10.0.0-nightly.20200403](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200403)|2020-04-03||yes|82|12.16.1|83.0.4087.0|128|
|[v10.0.0-nightly.20200402](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200402)|2020-04-02||yes|82|12.16.1|83.0.4087.0|253|
|[v10.0.0-nightly.20200401](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200401)|2020-04-01||yes|82|12.16.1|83.0.4087.0|171|
|[v10.0.0-nightly.20200331](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200331)|2020-03-31||yes|82|12.16.1|83.0.4087.0|199|
|[v10.0.0-nightly.20200330](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200330)|2020-03-30||yes|82|12.16.1|83.0.4087.0|164|
|[v10.0.0-nightly.20200327](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200327)|2020-03-27||yes|82|12.16.1|83.0.4087.0|220|
|[v10.0.0-nightly.20200326](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200326)|2020-03-26||yes|82|12.16.1|83.0.4087.0|161|
|[v10.0.0-nightly.20200325](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200325)|2020-03-25||yes|82|12.16.1|83.0.4087.0|158|
|[v10.0.0-nightly.20200324](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200324)|2020-03-24||yes|82|12.16.1|83.0.4087.0|184|
|[v10.0.0-nightly.20200323](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200323)|2020-03-23||yes|82|12.16.1|83.0.4087.0|173|
|[v10.0.0-nightly.20200320](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200320)|2020-03-20||yes|82|12.16.1|83.0.4087.0|218|
|[v10.0.0-nightly.20200318](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200318)|2020-03-18||yes|82|12.16.1|83.0.4087.0|158|
|[v10.0.0-nightly.20200317](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200317)|2020-03-17||yes|82|12.16.1|83.0.4087.0|243|
|[v10.0.0-nightly.20200316](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200316)|2020-03-16||yes|82|12.16.1|83.0.4086.0|123|
|[v10.0.0-nightly.20200311](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200311)|2020-03-11||yes|82|12.16.1|82.0.4083.0|126|
|[v10.0.0-nightly.20200310](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200310)|2020-03-10||yes|82|12.16.1|82.0.4076.0|360|
|[v10.0.0-nightly.20200309](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200309)|2020-03-09||yes|82|12.16.1|82.0.4076.0|163|
|[v10.0.0-nightly.20200306](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200306)|2020-03-06||yes|82|12.16.1|82.0.4076.0|229|
|[v10.0.0-nightly.20200305](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200305)|2020-03-05||yes|82|12.16.1|82.0.4076.0|160|
|[v10.0.0-nightly.20200304](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200304)|2020-03-04||yes|82|12.16.1|82.0.4076.0|161|
|[v10.0.0-nightly.20200303](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200303)|2020-03-03||yes|82|12.16.1|82.0.4050.0|163|
|[v10.0.0-nightly.20200226](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200226)|2020-02-26||yes|82|12.16.1|82.0.4050.0|376|
|[v10.0.0-nightly.20200223](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200223)|2020-02-23||yes|82|12.15.0|82.0.4050.0|251|
|[v10.0.0-nightly.20200222](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200222)|2020-02-22||yes|82|12.15.0|82.0.4050.0|177|
|[v10.0.0-nightly.20200221](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200221)|2020-02-21||yes|82|12.15.0|82.0.4050.0|380|
|[v10.0.0-nightly.20200218](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200218)|2020-02-18||yes|82|12.15.0|82.0.4050.0|252|
|[v10.0.0-nightly.20200217](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200217)|2020-02-17||yes|82|12.15.0|82.0.4050.0|183|
|[v10.0.0-nightly.20200216](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200216)|2020-02-16||yes|82|12.15.0|82.0.4050.0|250|
|[v10.0.0-nightly.20200211](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200211)|2020-02-11||yes|76|12.15.0|82.0.4050.0|302|
|[v10.0.0-nightly.20200210](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200210)|2020-02-10||yes|76|12.14.1|82.0.4050.0|690|
|[v10.0.0-nightly.20200209](https://github.com/electron/electron/releases/tag/v10.0.0-nightly.20200209)|2020-02-09||yes|76|12.14.1|82.0.4050.0|213|
|[v10.0.0-beta.25](https://github.com/electron/electron/releases/tag/v10.0.0-beta.25)|2020-08-22|beta-10-x-y|yes|82|12.16.3|85.0.4183.80|13782|
|[v10.0.0-beta.24](https://github.com/electron/electron/releases/tag/v10.0.0-beta.24)|2020-08-20||yes|82|12.16.3|85.0.4183.78|1617|
|[v10.0.0-beta.23](https://github.com/electron/electron/releases/tag/v10.0.0-beta.23)|2020-08-17||yes|82|12.16.3|85.0.4183.70|2437|
|[v10.0.0-beta.21](https://github.com/electron/electron/releases/tag/v10.0.0-beta.21)|2020-08-11||yes|82|12.16.3|85.0.4183.39|6675|
|[v10.0.0-beta.20](https://github.com/electron/electron/releases/tag/v10.0.0-beta.20)|2020-08-10||yes|82|12.16.3|85.0.4183.39|1511|
|[v10.0.0-beta.19](https://github.com/electron/electron/releases/tag/v10.0.0-beta.19)|2020-08-06||yes|82|12.16.3|85.0.4183.39|1974|
|[v10.0.0-beta.17](https://github.com/electron/electron/releases/tag/v10.0.0-beta.17)|2020-08-03||yes|82|12.16.3|85.0.4183.39|1689|
|[v10.0.0-beta.15](https://github.com/electron/electron/releases/tag/v10.0.0-beta.15)|2020-07-29||yes|82|12.16.3|85.0.4183.39|2701|
|[v10.0.0-beta.14](https://github.com/electron/electron/releases/tag/v10.0.0-beta.14)|2020-07-27||yes|82|12.16.3|85.0.4183.39|1728|
|[v10.0.0-beta.13](https://github.com/electron/electron/releases/tag/v10.0.0-beta.13)|2020-07-23||yes|82|12.16.3|85.0.4183.39|1415|
|[v10.0.0-beta.12](https://github.com/electron/electron/releases/tag/v10.0.0-beta.12)|2020-07-16||yes|82|12.16.3|85.0.4183.26|3215|
|[v10.0.0-beta.11](https://github.com/electron/electron/releases/tag/v10.0.0-beta.11)|2020-07-13||yes|82|12.16.3|85.0.4183.20|1913|
|[v10.0.0-beta.10](https://github.com/electron/electron/releases/tag/v10.0.0-beta.10)|2020-07-09||yes|82|12.16.3|85.0.4183.19|2071|
|[v10.0.0-beta.9](https://github.com/electron/electron/releases/tag/v10.0.0-beta.9)|2020-07-06||yes|82|12.16.3|85.0.4181.1|1905|
|[v10.0.0-beta.8](https://github.com/electron/electron/releases/tag/v10.0.0-beta.8)|2020-07-02||yes|82|12.16.3|85.0.4181.1|1693|
|[v10.0.0-beta.4](https://github.com/electron/electron/releases/tag/v10.0.0-beta.4)|2020-06-18||yes|82|12.16.3|85.0.4161.2|2324|
|[v10.0.0-beta.3](https://github.com/electron/electron/releases/tag/v10.0.0-beta.3)|2020-06-15||yes|82|12.16.3|85.0.4161.2|1430|
|[v10.0.0-beta.2](https://github.com/electron/electron/releases/tag/v10.0.0-beta.2)|2020-06-01||yes|82|12.16.3|84.0.4129.0|2752|
|[v10.0.0-beta.1](https://github.com/electron/electron/releases/tag/v10.0.0-beta.1)|2020-05-22||yes|82|12.16.3|84.0.4129.0|8319|
|[v9.4.0](https://github.com/electron/electron/releases/tag/v9.4.0)|2020-12-12|9-x-y|no|80|12.14.1|83.0.4103.122|19421|
|[v9.3.5](https://github.com/electron/electron/releases/tag/v9.3.5)|2020-11-24||no|80|12.14.1|83.0.4103.122|56010|
|[v9.3.4](https://github.com/electron/electron/releases/tag/v9.3.4)|2020-11-10||no|80|12.14.1|83.0.4103.122|44406|
|[v9.3.3](https://github.com/electron/electron/releases/tag/v9.3.3)|2020-10-26||no|80|12.14.1|83.0.4103.122|88956|
|[v9.3.2](https://github.com/electron/electron/releases/tag/v9.3.2)|2020-10-05||no|80|12.14.1|83.0.4103.122|152774|
|[v9.3.1](https://github.com/electron/electron/releases/tag/v9.3.1)|2020-09-15||no|80|12.14.1|83.0.4103.122|152653|
|[v9.3.0](https://github.com/electron/electron/releases/tag/v9.3.0)|2020-09-03||no|80|12.14.1|83.0.4103.122|123365|
|[v9.2.1](https://github.com/electron/electron/releases/tag/v9.2.1)|2020-08-18||no|80|12.14.1|83.0.4103.122|219855|
|[v9.2.0](https://github.com/electron/electron/releases/tag/v9.2.0)|2020-08-07||no|80|12.14.1|83.0.4103.122|201865|
|[v9.1.2](https://github.com/electron/electron/releases/tag/v9.1.2)|2020-07-28||no|80|12.14.1|83.0.4103.122|161937|
|[v9.1.1](https://github.com/electron/electron/releases/tag/v9.1.1)|2020-07-20||no|80|12.14.1|83.0.4103.122|133671|
|[v9.1.0](https://github.com/electron/electron/releases/tag/v9.1.0)|2020-07-06||no|80|12.14.1|83.0.4103.122|292710|
|[v9.0.5](https://github.com/electron/electron/releases/tag/v9.0.5)|2020-06-22||no|80|12.14.1|83.0.4103.119|201485|
|[v9.0.4](https://github.com/electron/electron/releases/tag/v9.0.4)|2020-06-12||no|80|12.14.1|83.0.4103.104|169811|
|[v9.0.3](https://github.com/electron/electron/releases/tag/v9.0.3)|2020-06-08||no|80|12.14.1|83.0.4103.100|86761|
|[v9.0.2](https://github.com/electron/electron/releases/tag/v9.0.2)|2020-06-02||no|80|12.14.1|83.0.4103.94|110668|
|[v9.0.1](https://github.com/electron/electron/releases/tag/v9.0.1)|2020-06-01||no|80|12.14.1|83.0.4103.94|28557|
|[v9.0.0](https://github.com/electron/electron/releases/tag/v9.0.0)|2020-05-19||no|80|12.14.1|83.0.4103.64|457947|
|[v9.0.0-nightly.20200121](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20200121)|2020-01-21||yes|76|12.14.1|81.0.4030.0|801|
|[v9.0.0-nightly.20200119](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20200119)|2020-01-19||yes|76|12.14.1|81.0.4030.0|217|
|[v9.0.0-nightly.20200117](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20200117)|2020-01-17||yes|76|12.14.1|81.0.3994.0|339|
|[v9.0.0-nightly.20200116](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20200116)|2020-01-16||yes|76|12.14.1|81.0.3994.0|290|
|[v9.0.0-nightly.20200115](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20200115)|2020-01-15||yes|76|12.14.1|81.0.3994.0|249|
|[v9.0.0-nightly.20200113](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20200113)|2020-01-13||yes|76|12.14.1|81.0.3994.0|261|
|[v9.0.0-nightly.20200111](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20200111)|2020-01-11||yes|76|12.14.1|81.0.3994.0|189|
|[v9.0.0-nightly.20200110](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20200110)|2020-01-10||yes|76|12.14.1|81.0.3994.0|271|
|[v9.0.0-nightly.20200109](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20200109)|2020-01-09||yes|76|12.14.0|81.0.3994.0|221|
|[v9.0.0-nightly.20200108](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20200108)|2020-01-08||yes|76|12.14.0|81.0.3994.0|227|
|[v9.0.0-nightly.20200106](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20200106)|2020-01-06||yes|76|12.14.0|81.0.3994.0|202|
|[v9.0.0-nightly.20200105](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20200105)|2020-01-05||yes|76|12.14.0|81.0.3994.0|191|
|[v9.0.0-nightly.20200104](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20200104)|2020-01-04||yes|76|12.14.0|81.0.3994.0|306|
|[v9.0.0-nightly.20200103](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20200103)|2020-01-03||yes|76|12.14.0|81.0.3994.0|206|
|[v9.0.0-nightly.20200101](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20200101)|2020-01-01||yes|76|12.14.0|81.0.3994.0|304|
|[v9.0.0-nightly.20191231](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191231)|2019-12-31||yes|76|12.14.0|81.0.3994.0|206|
|[v9.0.0-nightly.20191230](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191230)|2019-12-30||yes|76|12.14.0|81.0.3994.0|216|
|[v9.0.0-nightly.20191229](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191229)|2019-12-29||yes|76|12.14.0|81.0.3994.0|220|
|[v9.0.0-nightly.20191228](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191228)|2019-12-28||yes|76|12.14.0|81.0.3994.0|207|
|[v9.0.0-nightly.20191226](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191226)|2019-12-26||yes|76|12.14.0|81.0.3994.0|197|
|[v9.0.0-nightly.20191225](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191225)|2019-12-25||yes|76|12.14.0|81.0.3994.0|330|
|[v9.0.0-nightly.20191224](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191224)|2019-12-24||yes|76|12.14.0|81.0.3994.0|205|
|[v9.0.0-nightly.20191223](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191223)|2019-12-23||yes|76|12.14.0|81.0.3994.0|234|
|[v9.0.0-nightly.20191222](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191222)|2019-12-22||yes|76|12.14.0|81.0.3994.0|215|
|[v9.0.0-nightly.20191221](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191221)|2019-12-21||yes|76|12.14.0|81.0.3994.0|228|
|[v9.0.0-nightly.20191220](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191220)|2019-12-20||yes|76|12.14.0|81.0.3994.0|217|
|[v9.0.0-nightly.20191210](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191210)|2019-12-10||yes|76|12.13.0|80.0.3954.0|193|
|[v9.0.0-nightly.20191204](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191204)|2019-12-04||yes|76|12.13.0|80.0.3954.0|585|
|[v9.0.0-nightly.20191203](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191203)|2019-12-03||yes|76|12.13.0|80.0.3954.0|220|
|[v9.0.0-nightly.20191202](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191202)|2019-12-02||yes|76|12.13.0|80.0.3954.0|278|
|[v9.0.0-nightly.20191201](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191201)|2019-12-01||yes|76|12.13.0|80.0.3954.0|222|
|[v9.0.0-nightly.20191130](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191130)|2019-11-30||yes|76|12.13.0|80.0.3954.0|223|
|[v9.0.0-nightly.20191129](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191129)|2019-11-29||yes|76|12.13.0|80.0.3954.0|239|
|[v9.0.0-nightly.20191124](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191124)|2019-11-24||yes|76|12.13.0|80.0.3954.0|306|
|[v9.0.0-nightly.20191123](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191123)|2019-11-23||yes|76|12.13.0|80.0.3954.0|224|
|[v9.0.0-nightly.20191122](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191122)|2019-11-22||yes|76|12.13.0|80.0.3954.0|230|
|[v9.0.0-nightly.20191121](https://github.com/electron/electron/releases/tag/v9.0.0-nightly.20191121)|2019-11-21||yes|76|12.13.0|80.0.3954.0|235|
|[v9.0.0-beta.24](https://github.com/electron/electron/releases/tag/v9.0.0-beta.24)|2020-05-11|beta-9-x-y|yes|80|12.14.1|83.0.4103.45|13251|
|[v9.0.0-beta.23](https://github.com/electron/electron/releases/tag/v9.0.0-beta.23)|2020-05-07||yes|80|12.14.1|83.0.4103.44|1371|
|[v9.0.0-beta.22](https://github.com/electron/electron/releases/tag/v9.0.0-beta.22)|2020-04-30||yes|80|12.14.1|83.0.4103.34|5736|
|[v9.0.0-beta.21](https://github.com/electron/electron/releases/tag/v9.0.0-beta.21)|2020-04-28||yes|80|12.14.1|83.0.4103.26|1561|
|[v9.0.0-beta.20](https://github.com/electron/electron/releases/tag/v9.0.0-beta.20)|2020-04-27||yes|80|12.14.1|83.0.4103.26|1264|
|[v9.0.0-beta.19](https://github.com/electron/electron/releases/tag/v9.0.0-beta.19)|2020-04-24||yes|80|12.14.1|83.0.4103.24|2202|
|[v9.0.0-beta.18](https://github.com/electron/electron/releases/tag/v9.0.0-beta.18)|2020-04-20||yes|80|12.14.1|83.0.4103.16|2113|
|[v9.0.0-beta.17](https://github.com/electron/electron/releases/tag/v9.0.0-beta.17)|2020-04-17||yes|80|12.14.1|83.0.4103.14|1702|
|[v9.0.0-beta.16](https://github.com/electron/electron/releases/tag/v9.0.0-beta.16)|2020-04-13||yes|80|12.14.1|83.0.4102.3|1907|
|[v9.0.0-beta.15](https://github.com/electron/electron/releases/tag/v9.0.0-beta.15)|2020-04-09||yes|80|12.14.1|83.0.4102.3|2410|
|[v9.0.0-beta.14](https://github.com/electron/electron/releases/tag/v9.0.0-beta.14)|2020-04-06||yes|80|12.14.1|82.0.4085.27|1953|
|[v9.0.0-beta.13](https://github.com/electron/electron/releases/tag/v9.0.0-beta.13)|2020-04-02||yes|80|12.14.1|82.0.4085.14|4808|
|[v9.0.0-beta.12](https://github.com/electron/electron/releases/tag/v9.0.0-beta.12)|2020-03-30||yes|80|12.14.1|82.0.4085.14|1899|
|[v9.0.0-beta.10](https://github.com/electron/electron/releases/tag/v9.0.0-beta.10)|2020-03-23||yes|80|12.14.1|82.0.4085.10|2335|
|[v9.0.0-beta.9](https://github.com/electron/electron/releases/tag/v9.0.0-beta.9)|2020-03-16||yes|80|12.14.1|82.0.4058.2|2694|
|[v9.0.0-beta.7](https://github.com/electron/electron/releases/tag/v9.0.0-beta.7)|2020-03-09||yes|80|12.14.1|82.0.4058.2|2267|
|[v9.0.0-beta.6](https://github.com/electron/electron/releases/tag/v9.0.0-beta.6)|2020-03-06||yes|80|12.14.1|82.0.4058.2|2790|
|[v9.0.0-beta.5](https://github.com/electron/electron/releases/tag/v9.0.0-beta.5)|2020-03-02||yes|80|12.14.1|82.0.4048.0|3998|
|[v9.0.0-beta.4](https://github.com/electron/electron/releases/tag/v9.0.0-beta.4)|2020-02-28||yes|80|12.14.1|82.0.4048.0|1915|
|[v9.0.0-beta.3](https://github.com/electron/electron/releases/tag/v9.0.0-beta.3)|2020-02-26||yes|80|12.14.1|82.0.4048.0|1715|
|[v9.0.0-beta.2](https://github.com/electron/electron/releases/tag/v9.0.0-beta.2)|2020-02-16||yes|80|12.14.1|82.0.4048.0|2241|
|[v9.0.0-beta.1](https://github.com/electron/electron/releases/tag/v9.0.0-beta.1)|2020-02-10||yes|76|12.14.1|82.0.4048.0|1977|
|[v8.5.5](https://github.com/electron/electron/releases/tag/v8.5.5)|2020-11-18|8-x-y|no|76|12.13.0|80.0.3987.163|31850|
|[v8.5.4](https://github.com/electron/electron/releases/tag/v8.5.4)|2020-11-17||no|76|12.13.0|80.0.3987.163|3405|
|[v8.5.3](https://github.com/electron/electron/releases/tag/v8.5.3)|2020-10-30||no|76|12.13.0|80.0.3987.163|31556|
|[v8.5.2](https://github.com/electron/electron/releases/tag/v8.5.2)|2020-09-22||no|76|12.13.0|80.0.3987.165|102843|
|[v8.5.1](https://github.com/electron/electron/releases/tag/v8.5.1)|2020-09-03||no|76|12.13.0|80.0.3987.165|48728|
|[v8.5.0](https://github.com/electron/electron/releases/tag/v8.5.0)|2020-08-07||no|76|12.13.0|80.0.3987.165|89076|
|[v8.4.1](https://github.com/electron/electron/releases/tag/v8.4.1)|2020-07-23||no|76|12.13.0|80.0.3987.165|86917|
|[v8.4.0](https://github.com/electron/electron/releases/tag/v8.4.0)|2020-07-07||no|76|12.13.0|80.0.3987.165|56914|
|[v8.3.4](https://github.com/electron/electron/releases/tag/v8.3.4)|2020-06-25||no|76|12.13.0|80.0.3987.165|64842|
|[v8.3.3](https://github.com/electron/electron/releases/tag/v8.3.3)|2020-06-18||no|76|12.13.0|80.0.3987.165|68739|
|[v8.3.2](https://github.com/electron/electron/releases/tag/v8.3.2)|2020-06-15||no|76|12.13.0|80.0.3987.165|22691|
|[v8.3.1](https://github.com/electron/electron/releases/tag/v8.3.1)|2020-06-01||no|76|12.13.0|80.0.3987.165|71002|
|[v8.3.0](https://github.com/electron/electron/releases/tag/v8.3.0)|2020-05-15||no|76|12.13.0|80.0.3987.165|180697|
|[v8.2.5](https://github.com/electron/electron/releases/tag/v8.2.5)|2020-04-30||no|76|12.13.0|80.0.3987.165|313389|
|[v8.2.4](https://github.com/electron/electron/releases/tag/v8.2.4)|2020-04-28||no|76|12.13.0|80.0.3987.165|98542|
|[v8.2.3](https://github.com/electron/electron/releases/tag/v8.2.3)|2020-04-16||no|76|12.13.0|80.0.3987.163|210310|
|[v8.2.2](https://github.com/electron/electron/releases/tag/v8.2.2)|2020-04-13||no|76|12.13.0|80.0.3987.163|82092|
|[v8.2.1](https://github.com/electron/electron/releases/tag/v8.2.1)|2020-04-06||no|76|12.13.0|80.0.3987.163|104976|
|[v8.2.0](https://github.com/electron/electron/releases/tag/v8.2.0)|2020-03-24||no|76|12.13.0|80.0.3987.158|280559|
|[v8.1.1](https://github.com/electron/electron/releases/tag/v8.1.1)|2020-03-10||no|76|12.13.0|80.0.3987.141|209937|
|[v8.1.0](https://github.com/electron/electron/releases/tag/v8.1.0)|2020-03-06||no|76|12.13.0|80.0.3987.137|48292|
|[v8.0.3](https://github.com/electron/electron/releases/tag/v8.0.3)|2020-03-03||no|76|12.13.0|80.0.3987.134|85437|
|[v8.0.2](https://github.com/electron/electron/releases/tag/v8.0.2)|2020-02-26||no|76|12.13.0|80.0.3987.86|172950|
|[v8.0.1](https://github.com/electron/electron/releases/tag/v8.0.1)|2020-02-14||no|76|12.13.0|80.0.3987.86|173384|
|[v8.0.0](https://github.com/electron/electron/releases/tag/v8.0.0)|2020-02-03||no|76|12.13.0|80.0.3987.86|491091|
|[v8.0.0-nightly.20191105](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20191105)|2019-11-05||yes|76|12.13.0|80.0.3952.0|448|
|[v8.0.0-nightly.20191101](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20191101)|2019-11-01||yes|76|12.13.0|80.0.3952.0|289|
|[v8.0.0-nightly.20191023](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20191023)|2019-10-23||yes|76|12.13.0|79.0.3931.0|426|
|[v8.0.0-nightly.20191021](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20191021)|2019-10-21||yes|76|12.12.0|79.0.3931.0|264|
|[v8.0.0-nightly.20191020](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20191020)|2019-10-20||yes|76|12.12.0|79.0.3931.0|278|
|[v8.0.0-nightly.20191019](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20191019)|2019-10-19||yes|76|12.12.0|79.0.3931.0|256|
|[v8.0.0-nightly.20191017](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20191017)|2019-10-17||yes|76|12.10.0|79.0.3919.0|210|
|[v8.0.0-nightly.20191012](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20191012)|2019-10-12||yes|76|12.9.1|79.0.3919.0|277|
|[v8.0.0-nightly.20191011](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20191011)|2019-10-11||yes|76|12.9.1|79.0.3919.0|203|
|[v8.0.0-nightly.20191009](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20191009)|2019-10-09||yes|76|12.9.1|79.0.3919.0|226|
|[v8.0.0-nightly.20191006](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20191006)|2019-10-06||yes|76|12.9.1|79.0.3919.0|219|
|[v8.0.0-nightly.20191005](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20191005)|2019-10-05||yes|76|12.9.1|79.0.3919.0|205|
|[v8.0.0-nightly.20191004](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20191004)|2019-10-04||yes|76|12.9.1|79.0.3919.0|191|
|[v8.0.0-nightly.20191001](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20191001)|2019-10-01||yes|76|12.9.1|79.0.3919.0|247|
|[v8.0.0-nightly.20190930](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190930)|2019-09-30||yes|76|12.9.1|79.0.3919.0|204|
|[v8.0.0-nightly.20190929](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190929)|2019-09-29||yes|76|12.9.1|79.0.3919.0|211|
|[v8.0.0-nightly.20190926](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190926)|2019-09-26||yes|76|12.9.1|79.0.3919.0|239|
|[v8.0.0-nightly.20190924](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190924)|2019-09-24||yes|76|12.9.1|79.0.3919.0|199|
|[v8.0.0-nightly.20190923](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190923)|2019-09-23||yes|76|12.9.1|79.0.3919.0|200|
|[v8.0.0-nightly.20190920](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190920)|2019-09-20||yes|76|12.9.1|79.0.3915.0|261|
|[v8.0.0-nightly.20190919](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190919)|2019-09-19||yes|76|12.9.1|79.0.3915.0|226|
|[v8.0.0-nightly.20190917](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190917)|2019-09-17||yes|76|12.9.1|78.0.3892.0|213|
|[v8.0.0-nightly.20190915](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190915)|2019-09-15||yes|76|12.9.1|78.0.3892.0|184|
|[v8.0.0-nightly.20190914](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190914)|2019-09-14||yes|76|12.9.1|78.0.3892.0|210|
|[v8.0.0-nightly.20190913](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190913)|2019-09-13||yes|76|12.9.1|78.0.3892.0|195|
|[v8.0.0-nightly.20190911](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190911)|2019-09-11||yes|76|12.9.1|78.0.3892.0|192|
|[v8.0.0-nightly.20190910](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190910)|2019-09-10||yes|76|12.9.1|78.0.3892.0|202|
|[v8.0.0-nightly.20190909](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190909)|2019-09-09||yes|76|12.9.1|78.0.3892.0|207|
|[v8.0.0-nightly.20190907](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190907)|2019-09-08||yes|76|12.9.1|78.0.3892.0|184|
|[v8.0.0-nightly.20190902](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190902)|2019-09-02||yes|76|12.9.1|78.0.3892.0|165|
|[v8.0.0-nightly.20190901](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190901)|2019-09-01||yes|76|12.9.1|78.0.3892.0|292|
|[v8.0.0-nightly.20190830](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190830)|2019-08-30||yes|76|12.9.1|78.0.3892.0|191|
|[v8.0.0-nightly.20190828](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190828)|2019-08-28||yes|76|12.9.1|78.0.3892.0|141|
|[v8.0.0-nightly.20190827](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190827)|2019-08-28||yes|76|12.9.1|78.0.3892.0|114|
|[v8.0.0-nightly.20190825](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190825)|2019-08-25||yes|76|12.9.0|78.0.3892.0|208|
|[v8.0.0-nightly.20190824](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190824)|2019-08-24||yes|76|12.9.0|78.0.3892.0|165|
|[v8.0.0-nightly.20190820](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190820)|2019-08-20||yes|76|12.8.1|78.0.3881.0|162|
|[v8.0.0-nightly.20190819](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190819)|2019-08-20||yes|76|12.8.0|78.0.3881.0|221|
|[v8.0.0-nightly.20190818](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190818)|2019-08-18||yes|76|12.8.0|78.0.3881.0|185|
|[v8.0.0-nightly.20190817](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190817)|2019-08-17||yes|76|12.8.0|78.0.3881.0|174|
|[v8.0.0-nightly.20190816](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190816)|2019-08-16||yes|76|12.8.0|78.0.3881.0|177|
|[v8.0.0-nightly.20190815](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190815)|2019-08-15||yes|76|12.8.0|78.0.3871.0|172|
|[v8.0.0-nightly.20190814](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190814)|2019-08-14||yes|76|12.8.0|78.0.3871.0|168|
|[v8.0.0-nightly.20190813](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190813)|2019-08-13||yes|76|12.8.0|78.0.3871.0|186|
|[v8.0.0-nightly.20190812](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190812)|2019-08-12||yes|76|12.6.0|78.0.3871.0|181|
|[v8.0.0-nightly.20190811](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190811)|2019-08-12||yes|76|12.6.0|78.0.3871.0|178|
|[v8.0.0-nightly.20190810](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190810)|2019-08-11||yes|76|12.6.0|78.0.3871.0|167|
|[v8.0.0-nightly.20190809](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190809)|2019-08-09||yes|76|12.6.0|78.0.3871.0|171|
|[v8.0.0-nightly.20190808](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190808)|2019-08-08||yes|76|12.6.0|78.0.3871.0|167|
|[v8.0.0-nightly.20190807](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190807)|2019-08-07||yes|76|12.6.0|78.0.3871.0|182|
|[v8.0.0-nightly.20190806](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190806)|2019-08-06||yes|76|12.6.0|78.0.3871.0|187|
|[v8.0.0-nightly.20190803](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190803)|2019-08-04||yes|76|12.6.0|78.0.3871.0|232|
|[v8.0.0-nightly.20190802](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190802)|2019-08-02||yes|76|12.6.0|78.0.3866.0|180|
|[v8.0.0-nightly.20190801](https://github.com/electron/electron/releases/tag/v8.0.0-nightly.20190801)|2019-08-01||yes|76|12.6.0|78.0.3866.0|181|
|[v8.0.0-beta.9](https://github.com/electron/electron/releases/tag/v8.0.0-beta.9)|2020-01-30|beta-8-x-y|yes|76|12.13.0|80.0.3987.75|4560|
|[v8.0.0-beta.8](https://github.com/electron/electron/releases/tag/v8.0.0-beta.8)|2020-01-29||yes|76|12.13.0|80.0.3987.75|1355|
|[v8.0.0-beta.7](https://github.com/electron/electron/releases/tag/v8.0.0-beta.7)|2020-01-16||yes|76|12.13.0|80.0.3987.59|59032|
|[v8.0.0-beta.6](https://github.com/electron/electron/releases/tag/v8.0.0-beta.6)|2020-01-14||yes|76|12.13.0|80.0.3987.51|2135|
|[v8.0.0-beta.5](https://github.com/electron/electron/releases/tag/v8.0.0-beta.5)|2019-12-18||yes|76|12.13.0|80.0.3987.14|40037|
|[v8.0.0-beta.4](https://github.com/electron/electron/releases/tag/v8.0.0-beta.4)|2019-12-04||yes|76|12.13.0|80.0.3955.0|29869|
|[v8.0.0-beta.3](https://github.com/electron/electron/releases/tag/v8.0.0-beta.3)|2019-11-20||yes|76|12.13.0|80.0.3955.0|7281|
|[v8.0.0-beta.2](https://github.com/electron/electron/releases/tag/v8.0.0-beta.2)|2019-11-01||yes|76|12.13.0|79.0.3931.0|5932|
|[v8.0.0-beta.1](https://github.com/electron/electron/releases/tag/v8.0.0-beta.1)|2019-10-24||yes|76|12.13.0|79.0.3931.0|2501|
|[v7.3.3](https://github.com/electron/electron/releases/tag/v7.3.3)|2020-08-25|7-3-x|no|75|12.8.1|78.0.3904.130|126122|
|[v7.3.2](https://github.com/electron/electron/releases/tag/v7.3.2)|2020-06-24||no|75|12.8.1|78.0.3904.130|179429|
|[v7.3.1](https://github.com/electron/electron/releases/tag/v7.3.1)|2020-06-01||no|75|12.8.1|78.0.3904.130|78644|
|[v7.3.0](https://github.com/electron/electron/releases/tag/v7.3.0)|2020-05-15||no|75|12.8.1|78.0.3904.130|52021|
|[v7.2.4](https://github.com/electron/electron/releases/tag/v7.2.4)|2020-04-29|7-2-x|no|75|12.8.1|78.0.3904.130|138701|
|[v7.2.3](https://github.com/electron/electron/releases/tag/v7.2.3)|2020-04-17||no|75|12.8.1|78.0.3904.130|34264|
|[v7.2.2](https://github.com/electron/electron/releases/tag/v7.2.2)|2020-04-14||no|75|12.8.1|78.0.3904.130|24029|
|[v7.2.1](https://github.com/electron/electron/releases/tag/v7.2.1)|2020-03-24||no|75|12.8.1|78.0.3904.130|123756|
|[v7.2.0](https://github.com/electron/electron/releases/tag/v7.2.0)|2020-03-23||no|75|12.8.1|78.0.3904.130|3748|
|[v7.1.14](https://github.com/electron/electron/releases/tag/v7.1.14)|2020-02-28|7-1-x|no|75|12.8.1|78.0.3904.130|164087|
|[v7.1.13](https://github.com/electron/electron/releases/tag/v7.1.13)|2020-02-20||no|75|12.8.1|78.0.3904.130|105976|
|[v7.1.12](https://github.com/electron/electron/releases/tag/v7.1.12)|2020-02-10||no|75|12.8.1|78.0.3904.130|76270|
|[v7.1.11](https://github.com/electron/electron/releases/tag/v7.1.11)|2020-01-30||no|75|12.8.1|78.0.3904.130|359634|
|[v7.1.10](https://github.com/electron/electron/releases/tag/v7.1.10)|2020-01-22||no|75|12.8.1|78.0.3904.130|184342|
|[v7.1.9](https://github.com/electron/electron/releases/tag/v7.1.9)|2020-01-13||no|75|12.8.1|78.0.3904.130|210899|
|[v7.1.8](https://github.com/electron/electron/releases/tag/v7.1.8)|2020-01-08||no|75|12.8.1|78.0.3904.130|108642|
|[v7.1.7](https://github.com/electron/electron/releases/tag/v7.1.7)|2019-12-19||no|75|12.8.1|78.0.3904.130|382211|
|[v7.1.6](https://github.com/electron/electron/releases/tag/v7.1.6)|2019-12-18||no|75|12.8.1|78.0.3904.130|68564|
|[v7.1.5](https://github.com/electron/electron/releases/tag/v7.1.5)|2019-12-13||no|75|12.8.1|78.0.3904.130|91673|
|[v7.1.4](https://github.com/electron/electron/releases/tag/v7.1.4)|2019-12-10||no|75|12.8.1|78.0.3904.130|135139|
|[v7.1.3](https://github.com/electron/electron/releases/tag/v7.1.3)|2019-12-03||no|75|12.8.1|78.0.3904.126|144251|
|[v7.1.2](https://github.com/electron/electron/releases/tag/v7.1.2)|2019-11-20||no|75|12.8.1|78.0.3904.113|265395|
|[v7.1.1](https://github.com/electron/electron/releases/tag/v7.1.1)|2019-11-07||no|75|12.8.1|78.0.3904.99|238992|
|[v7.1.0](https://github.com/electron/electron/releases/tag/v7.1.0)|2019-11-05||no|75|12.8.1|78.0.3904.94|88497|
|[v7.0.1](https://github.com/electron/electron/releases/tag/v7.0.1)|2019-11-01|7-0-x|no|75|12.8.1|78.0.3904.92|116559|
|[v7.0.0](https://github.com/electron/electron/releases/tag/v7.0.0)|2019-10-21||no|75|12.8.1|78.0.3905.1|669850|
|[v7.0.0-nightly.20190731](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190731)|2019-07-31||yes|75|12.6.0|78.0.3866.0|222|
|[v7.0.0-nightly.20190730](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190730)|2019-07-30||yes|75|12.6.0|78.0.3866.0|215|
|[v7.0.0-nightly.20190729](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190729)|2019-07-29||yes|75|12.6.0|78.0.3866.0|171|
|[v7.0.0-nightly.20190728](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190728)|2019-07-28||yes|75|12.6.0|78.0.3866.0|176|
|[v7.0.0-nightly.20190727](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190727)|2019-07-27||yes|75|12.6.0|78.0.3866.0|187|
|[v7.0.0-nightly.20190726](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190726)|2019-07-26||yes|75|12.6.0|77.0.3864.0|185|
|[v7.0.0-nightly.20190721](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190721)|2019-07-21||yes|73|12.6.0|77.0.3848.0|451|
|[v7.0.0-nightly.20190720](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190720)|2019-07-20||yes|73|12.6.0|77.0.3848.0|199|
|[v7.0.0-nightly.20190719](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190719)|2019-07-19||yes|73|12.6.0|77.0.3848.0|179|
|[v7.0.0-nightly.20190705](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190705)|2019-07-05||yes|73|12.0.0|77.0.3843.0|176|
|[v7.0.0-nightly.20190704](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190704)|2019-07-04||yes|73|12.0.0|77.0.3843.0|402|
|[v7.0.0-nightly.20190702](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190702)|2019-07-02||yes|73|12.0.0|77.0.3815.0|203|
|[v7.0.0-nightly.20190701](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190701)|2019-07-01||yes|73|12.0.0|77.0.3815.0|171|
|[v7.0.0-nightly.20190630](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190630)|2019-07-01||yes|73|12.0.0|77.0.3815.0|174|
|[v7.0.0-nightly.20190629](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190629)|2019-06-29||yes|73|12.0.0|77.0.3815.0|183|
|[v7.0.0-nightly.20190627](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190627)|2019-06-27||yes|73|12.0.0|77.0.3815.0|197|
|[v7.0.0-nightly.20190624](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190624)|2019-06-24||yes|73|12.0.0|77.0.3815.0|217|
|[v7.0.0-nightly.20190623](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190623)|2019-06-23||yes|73|12.0.0|77.0.3815.0|166|
|[v7.0.0-nightly.20190622](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190622)|2019-06-22||yes|73|12.0.0|77.0.3815.0|175|
|[v7.0.0-nightly.20190619](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190619)|2019-06-19||yes|73|12.0.0|77.0.3815.0|209|
|[v7.0.0-nightly.20190618](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190618)|2019-06-18||yes|73|12.0.0|77.0.3815.0|171|
|[v7.0.0-nightly.20190616](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190616)|2019-06-16||yes|73|12.0.0|77.0.3815.0|197|
|[v7.0.0-nightly.20190615](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190615)|2019-06-15||yes|73|12.0.0|77.0.3815.0|179|
|[v7.0.0-nightly.20190613](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190613)|2019-06-13||yes|73|12.0.0|77.0.3815.0|199|
|[v7.0.0-nightly.20190612](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190612)|2019-06-12||yes|73|12.0.0|77.0.3815.0|188|
|[v7.0.0-nightly.20190611](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190611)|2019-06-11||yes|73|12.0.0|77.0.3815.0|165|
|[v7.0.0-nightly.20190609](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190609)|2019-06-09||yes|73|12.0.0|77.0.3815.0|166|
|[v7.0.0-nightly.20190608](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190608)|2019-06-08||yes|73|12.0.0|77.0.3815.0|184|
|[v7.0.0-nightly.20190607](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190607)|2019-06-07||yes|73|12.0.0|77.0.3815.0|174|
|[v7.0.0-nightly.20190606](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190606)|2019-06-06||yes|73|12.0.0|77.0.3815.0|178|
|[v7.0.0-nightly.20190605](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190605)|2019-06-05||yes|73|12.0.0|77.0.3815.0|182|
|[v7.0.0-nightly.20190604](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190604)|2019-06-04||yes|73|12.0.0|77.0.3814.0|169|
|[v7.0.0-nightly.20190603](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190603)|2019-06-03||yes|73|12.0.0|76.0.3806.0|172|
|[v7.0.0-nightly.20190602](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190602)|2019-06-02||yes|73|12.0.0|76.0.3806.0|169|
|[v7.0.0-nightly.20190531](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190531)|2019-05-31||yes|73|12.0.0|76.0.3806.0|174|
|[v7.0.0-nightly.20190530](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190530)|2019-05-30||yes|73|12.0.0|76.0.3806.0|175|
|[v7.0.0-nightly.20190529](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190529)|2019-05-29||yes|73|12.0.0|76.0.3806.0|179|
|[v7.0.0-nightly.20190521](https://github.com/electron/electron/releases/tag/v7.0.0-nightly.20190521)|2019-05-21||yes|73|12.0.0|76.0.3784.0|560|
|[v7.0.0-beta.7](https://github.com/electron/electron/releases/tag/v7.0.0-beta.7)|2019-10-16|beta-7-0-x|yes|75|12.8.1|78.0.3905.1|2678|
|[v7.0.0-beta.6](https://github.com/electron/electron/releases/tag/v7.0.0-beta.6)|2019-10-09||yes|75|12.8.1|78.0.3905.1|2813|
|[v7.0.0-beta.5](https://github.com/electron/electron/releases/tag/v7.0.0-beta.5)|2019-09-24||yes|75|12.8.1|78.0.3905.1|4345|
|[v7.0.0-beta.4](https://github.com/electron/electron/releases/tag/v7.0.0-beta.4)|2019-09-03||yes|75|12.8.1|78.0.3896.6|3644|
|[v7.0.0-beta.3](https://github.com/electron/electron/releases/tag/v7.0.0-beta.3)|2019-08-15||yes|75|12.8.0|78.0.3866.0|3850|
|[v7.0.0-beta.2](https://github.com/electron/electron/releases/tag/v7.0.0-beta.2)|2019-08-07||yes|75|12.6.0|78.0.3866.0|2755|
|[v7.0.0-beta.1](https://github.com/electron/electron/releases/tag/v7.0.0-beta.1)|2019-08-01||yes|75|12.6.0|78.0.3866.0|2216|
|[v6.1.12](https://github.com/electron/electron/releases/tag/v6.1.12)|2020-05-18|6-1-x|no|73|12.4.0|76.0.3809.146|221362|
|[v6.1.11](https://github.com/electron/electron/releases/tag/v6.1.11)|2020-05-05||no|73|12.4.0|76.0.3809.146|40237|
|[v6.1.10](https://github.com/electron/electron/releases/tag/v6.1.10)|2020-04-14||no|73|12.4.0|76.0.3809.146|60949|
|[v6.1.9](https://github.com/electron/electron/releases/tag/v6.1.9)|2020-02-28||no|73|12.4.0|76.0.3809.146|84987|
|[v6.1.8](https://github.com/electron/electron/releases/tag/v6.1.8)|2020-02-21||no|73|12.4.0|76.0.3809.146|12561|
|[v6.1.7](https://github.com/electron/electron/releases/tag/v6.1.7)|2019-12-17||no|73|12.4.0|76.0.3809.146|329496|
|[v6.1.6](https://github.com/electron/electron/releases/tag/v6.1.6)|2019-12-11||no|73|12.4.0|76.0.3809.146|133124|
|[v6.1.5](https://github.com/electron/electron/releases/tag/v6.1.5)|2019-11-21||no|73|12.4.0|76.0.3809.146|142056|
|[v6.1.4](https://github.com/electron/electron/releases/tag/v6.1.4)|2019-11-05||no|73|12.4.0|76.0.3809.146|175654|
|[v6.1.3](https://github.com/electron/electron/releases/tag/v6.1.3)|2019-11-01||no|73|12.4.0|76.0.3809.146|17655|
|[v6.1.2](https://github.com/electron/electron/releases/tag/v6.1.2)|2019-10-24||no|73|12.4.0|76.0.3809.146|74862|
|[v6.1.1](https://github.com/electron/electron/releases/tag/v6.1.1)|2019-10-23||no|73|12.4.0|76.0.3809.146|15711|
|[v6.1.0](https://github.com/electron/electron/releases/tag/v6.1.0)|2019-10-22||no|73|12.4.0|76.0.3809.146|14044|
|[v6.0.12](https://github.com/electron/electron/releases/tag/v6.0.12)|2019-10-08|6-0-x|no|73|12.4.0|76.0.3809.146|270374|
|[v6.0.11](https://github.com/electron/electron/releases/tag/v6.0.11)|2019-10-02||no|73|12.4.0|76.0.3809.146|136765|
|[v6.0.10](https://github.com/electron/electron/releases/tag/v6.0.10)|2019-09-19||no|73|12.4.0|76.0.3809.146|319918|
|[v6.0.9](https://github.com/electron/electron/releases/tag/v6.0.9)|2019-09-12||no|73|12.4.0|76.0.3809.146|112202|
|[v6.0.8](https://github.com/electron/electron/releases/tag/v6.0.8)|2019-09-09||no|73|12.4.0|76.0.3809.146|75926|
|[v6.0.7](https://github.com/electron/electron/releases/tag/v6.0.7)|2019-08-31||no|73|12.4.0|76.0.3809.139|219465|
|[v6.0.6](https://github.com/electron/electron/releases/tag/v6.0.6)|2019-08-30||no|73|12.4.0|76.0.3809.138|15371|
|[v6.0.5](https://github.com/electron/electron/releases/tag/v6.0.5)|2019-08-27||no|73|12.4.0|76.0.3809.136|94669|
|[v6.0.4](https://github.com/electron/electron/releases/tag/v6.0.4)|2019-08-24||no|73|12.4.0|76.0.3809.131|42000|
|[v6.0.3](https://github.com/electron/electron/releases/tag/v6.0.3)|2019-08-20||no|73|12.4.0|76.0.3809.126|63915|
|[v6.0.2](https://github.com/electron/electron/releases/tag/v6.0.2)|2019-08-12||no|73|12.4.0|76.0.3809.110|80825|
|[v6.0.1](https://github.com/electron/electron/releases/tag/v6.0.1)|2019-08-07||no|73|12.4.0|76.0.3809.102|83785|
|[v6.0.0](https://github.com/electron/electron/releases/tag/v6.0.0)|2019-07-29||no|73|12.4.0|76.0.3809.88|489133|
|[v6.0.0-nightly.20190311](https://github.com/electron/electron/releases/tag/v6.0.0-nightly.20190311)|2019-03-11||yes|68|12.0.0|74.0.3724.8|3211|
|[v6.0.0-nightly.20190213](https://github.com/electron/electron/releases/tag/v6.0.0-nightly.20190213)|2019-02-14||yes|68|12.0.0|72.0.3626.110|1247|
|[v6.0.0-nightly.20190212](https://github.com/electron/electron/releases/tag/v6.0.0-nightly.20190212)|2019-02-12||yes|68|12.0.0|72.0.3626.107|194|
|[v6.0.0-beta.15](https://github.com/electron/electron/releases/tag/v6.0.0-beta.15)|2019-07-20|beta-6-0-x|yes|73|12.4.0|76.0.3809.74|4935|
|[v6.0.0-beta.14](https://github.com/electron/electron/releases/tag/v6.0.0-beta.14)|2019-07-17||yes|73|12.4.0|76.0.3809.68|2369|
|[v6.0.0-beta.13](https://github.com/electron/electron/releases/tag/v6.0.0-beta.13)|2019-07-10||yes|73|12.0.0|76.0.3809.60|3184|
|[v6.0.0-beta.12](https://github.com/electron/electron/releases/tag/v6.0.0-beta.12)|2019-07-04||yes|73|12.0.0|76.0.3809.54|3016|
|[v6.0.0-beta.11](https://github.com/electron/electron/releases/tag/v6.0.0-beta.11)|2019-06-26||yes|73|12.0.0|76.0.3809.42|2396|
|[v6.0.0-beta.10](https://github.com/electron/electron/releases/tag/v6.0.0-beta.10)|2019-06-22||yes|73|12.0.0|76.0.3809.37|2369|
|[v6.0.0-beta.9](https://github.com/electron/electron/releases/tag/v6.0.0-beta.9)|2019-06-18||yes|73|12.0.0|76.0.3809.26|2001|
|[v6.0.0-beta.8](https://github.com/electron/electron/releases/tag/v6.0.0-beta.8)|2019-06-14||yes|73|12.0.0|76.0.3809.26|1771|
|[v6.0.0-beta.7](https://github.com/electron/electron/releases/tag/v6.0.0-beta.7)|2019-06-11||yes|73|12.0.0|76.0.3809.22|1769|
|[v6.0.0-beta.6](https://github.com/electron/electron/releases/tag/v6.0.0-beta.6)|2019-06-05||yes|73|12.0.0|76.0.3809.3|2048|
|[v6.0.0-beta.5](https://github.com/electron/electron/releases/tag/v6.0.0-beta.5)|2019-05-29||yes|73|12.0.0|76.0.3805.4|1673|
|[v6.0.0-beta.4](https://github.com/electron/electron/releases/tag/v6.0.0-beta.4)|2019-05-21||yes|73|12.0.0|76.0.3783.1|2099|
|[v6.0.0-beta.3](https://github.com/electron/electron/releases/tag/v6.0.0-beta.3)|2019-05-14||yes|73|12.0.0|76.0.3783.1|13995|
|[v6.0.0-beta.2](https://github.com/electron/electron/releases/tag/v6.0.0-beta.2)|2019-05-08||yes|73|12.0.0|76.0.3783.1|2360|
|[v6.0.0-beta.1](https://github.com/electron/electron/releases/tag/v6.0.0-beta.1)|2019-05-01||yes|73|12.0.0|76.0.3774.1|2203|
|[v5.0.13](https://github.com/electron/electron/releases/tag/v5.0.13)|2019-12-17|5-0-x|no|70|12.0.0|73.0.3683.121|593015|
|[v5.0.12](https://github.com/electron/electron/releases/tag/v5.0.12)|2019-11-02||no|70|12.0.0|73.0.3683.121|163116|
|[v5.0.11](https://github.com/electron/electron/releases/tag/v5.0.11)|2019-09-24||no|70|12.0.0|73.0.3683.121|125638|
|[v5.0.10](https://github.com/electron/electron/releases/tag/v5.0.10)|2019-08-20||no|70|12.0.0|73.0.3683.121|215269|
|[v5.0.9](https://github.com/electron/electron/releases/tag/v5.0.9)|2019-08-06||no|70|12.0.0|73.0.3683.121|226490|
|[v5.0.8](https://github.com/electron/electron/releases/tag/v5.0.8)|2019-07-23||no|70|12.0.0|73.0.3683.121|178348|
|[v5.0.7](https://github.com/electron/electron/releases/tag/v5.0.7)|2019-07-16||no|70|12.0.0|73.0.3683.121|134584|
|[v5.0.6](https://github.com/electron/electron/releases/tag/v5.0.6)|2019-06-26||no|70|12.0.0|73.0.3683.121|319509|
|[v5.0.5](https://github.com/electron/electron/releases/tag/v5.0.5)|2019-06-21||no|70|12.0.0|73.0.3683.121|126654|
|[v5.0.4](https://github.com/electron/electron/releases/tag/v5.0.4)|2019-06-15||no|70|12.0.0|73.0.3683.121|142619|
|[v5.0.3](https://github.com/electron/electron/releases/tag/v5.0.3)|2019-06-07||no|70|12.0.0|73.0.3683.121|90845|
|[v5.0.2](https://github.com/electron/electron/releases/tag/v5.0.2)|2019-05-23||no|70|12.0.0|73.0.3683.121|207795|
|[v5.0.1](https://github.com/electron/electron/releases/tag/v5.0.1)|2019-05-04||no|70|12.0.0|73.0.3683.121|746428|
|[v5.0.0](https://github.com/electron/electron/releases/tag/v5.0.0)|2019-04-24||no|70|12.0.0|73.0.3683.119|167984|
|[v5.0.0-nightly.20190122](https://github.com/electron/electron/releases/tag/v5.0.0-nightly.20190122)|2019-01-22||yes|68|12.0.0|71.0.3578.98|151|
|[v5.0.0-nightly.20190121](https://github.com/electron/electron/releases/tag/v5.0.0-nightly.20190121)|2019-01-22||yes|68|12.0.0|71.0.3578.98|291|
|[v5.0.0-nightly.20190107](https://github.com/electron/electron/releases/tag/v5.0.0-nightly.20190107)|2019-01-08||yes|67|11.0.0|70.0.3538.110|645|
|[v5.0.0-beta.9](https://github.com/electron/electron/releases/tag/v5.0.0-beta.9)|2019-04-20|beta-5-0-x|yes|70|12.0.0|73.0.3683.117|3818|
|[v5.0.0-beta.8](https://github.com/electron/electron/releases/tag/v5.0.0-beta.8)|2019-04-04||yes|68|12.0.0|73.0.3683.104|8358|
|[v5.0.0-beta.7](https://github.com/electron/electron/releases/tag/v5.0.0-beta.7)|2019-03-28||yes|68|12.0.0|73.0.3683.94|20813|
|[v5.0.0-beta.6](https://github.com/electron/electron/releases/tag/v5.0.0-beta.6)|2019-03-20||yes|68|12.0.0|73.0.3683.84|49572|
|[v5.0.0-beta.5](https://github.com/electron/electron/releases/tag/v5.0.0-beta.5)|2019-03-05||yes|68|12.0.0|73.0.3683.61|6833|
|[v5.0.0-beta.4](https://github.com/electron/electron/releases/tag/v5.0.0-beta.4)|2019-02-27||yes|68|12.0.0|73.0.3683.54|2847|
|[v5.0.0-beta.3](https://github.com/electron/electron/releases/tag/v5.0.0-beta.3)|2019-02-15||yes|68|12.0.0|73.0.3683.27|5632|
|[v5.0.0-beta.2](https://github.com/electron/electron/releases/tag/v5.0.0-beta.2)|2019-02-04||yes|68|12.0.0|72.0.3626.52|5008|
|[v5.0.0-beta.1](https://github.com/electron/electron/releases/tag/v5.0.0-beta.1)|2019-01-23||yes|68|12.0.0|72.0.3626.52|8225|
|[v4.2.12](https://github.com/electron/electron/releases/tag/v4.2.12)|2019-10-16|4-2-x|no|69|10.11.0|69.0.3497.128|510804|
|[v4.2.11](https://github.com/electron/electron/releases/tag/v4.2.11)|2019-09-24||no|69|10.11.0|69.0.3497.128|62920|
|[v4.2.10](https://github.com/electron/electron/releases/tag/v4.2.10)|2019-08-29||no|69|10.11.0|69.0.3497.128|94872|
|[v4.2.9](https://github.com/electron/electron/releases/tag/v4.2.9)|2019-08-06||no|69|10.11.0|69.0.3497.128|92320|
|[v4.2.8](https://github.com/electron/electron/releases/tag/v4.2.8)|2019-07-20||no|69|10.11.0|69.0.3497.128|65315|
|[v4.2.7](https://github.com/electron/electron/releases/tag/v4.2.7)|2019-07-16||no|69|10.11.0|69.0.3497.128|31776|
|[v4.2.6](https://github.com/electron/electron/releases/tag/v4.2.6)|2019-07-02||no|69|10.11.0|69.0.3497.128|82033|
|[v4.2.5](https://github.com/electron/electron/releases/tag/v4.2.5)|2019-06-21||no|69|10.11.0|69.0.3497.128|64466|
|[v4.2.4](https://github.com/electron/electron/releases/tag/v4.2.4)|2019-06-05||no|69|10.11.0|69.0.3497.128|116223|
|[v4.2.3](https://github.com/electron/electron/releases/tag/v4.2.3)|2019-05-31||no|69|10.11.0|69.0.3497.128|60536|
|[v4.2.2](https://github.com/electron/electron/releases/tag/v4.2.2)|2019-05-17||no|69|10.11.0|69.0.3497.128|80919|
|[v4.2.1](https://github.com/electron/electron/releases/tag/v4.2.1)|2019-05-14||no|69|10.11.0|69.0.3497.128|32166|
|[v4.2.0](https://github.com/electron/electron/releases/tag/v4.2.0)|2019-05-03||no|69|10.11.0|69.0.3497.128|194719|
|[v4.1.5](https://github.com/electron/electron/releases/tag/v4.1.5)|2019-04-24|4-1-x|no|69|10.11.0|69.0.3497.128|72992|
|[v4.1.4](https://github.com/electron/electron/releases/tag/v4.1.4)|2019-04-04||no|69|10.11.0|69.0.3497.128|428347|
|[v4.1.3](https://github.com/electron/electron/releases/tag/v4.1.3)|2019-03-29||no|69|10.11.0|69.0.3497.128|119572|
|[v4.1.2](https://github.com/electron/electron/releases/tag/v4.1.2)|2019-03-28||no|69|10.11.0|69.0.3497.128|17300|
|[v4.1.1](https://github.com/electron/electron/releases/tag/v4.1.1)|2019-03-20||no|69|10.11.0|69.0.3497.128|132487|
|[v4.1.0](https://github.com/electron/electron/releases/tag/v4.1.0)|2019-03-14||no|69|10.11.0|69.0.3497.128|146883|
|[v4.0.8](https://github.com/electron/electron/releases/tag/v4.0.8)|2019-03-08|4-0-x|no|69|10.11.0|69.0.3497.128|96991|
|[v4.0.7](https://github.com/electron/electron/releases/tag/v4.0.7)|2019-03-05||no|69|10.11.0|69.0.3497.128|86386|
|[v4.0.6](https://github.com/electron/electron/releases/tag/v4.0.6)|2019-02-27||no|69|10.11.0|69.0.3497.106|106257|
|[v4.0.5](https://github.com/electron/electron/releases/tag/v4.0.5)|2019-02-15||no|69|10.11.0|69.0.3497.106|159141|
|[v4.0.4](https://github.com/electron/electron/releases/tag/v4.0.4)|2019-02-02||no|69|10.11.0|69.0.3497.106|308972|
|[v4.0.3](https://github.com/electron/electron/releases/tag/v4.0.3)|2019-01-29||no|64|10.11.0|69.0.3497.106|80622|
|[v4.0.2](https://github.com/electron/electron/releases/tag/v4.0.2)|2019-01-22||no|64|10.11.0|69.0.3497.106|116634|
|[v4.0.1](https://github.com/electron/electron/releases/tag/v4.0.1)|2019-01-04||no|64|10.11.0|69.0.3497.106|326254|
|[v4.0.0](https://github.com/electron/electron/releases/tag/v4.0.0)|2018-12-20||no|64|10.11.0|69.0.3497.106|517259|
|[v4.0.0-nightly.20181010](https://github.com/electron/electron/releases/tag/v4.0.0-nightly.20181010)|2018-10-10||yes|64|10.11.0|69.0.3497.106|8570|
|[v4.0.0-nightly.20181006](https://github.com/electron/electron/releases/tag/v4.0.0-nightly.20181006)|2018-10-06||yes|64|10.11.0|68.0.3440.128|591|
|[v4.0.0-nightly.20180929](https://github.com/electron/electron/releases/tag/v4.0.0-nightly.20180929)|2018-09-29||yes|64|10.6.0|67.0.3396.99|662|
|[v4.0.0-nightly.20180821](https://github.com/electron/electron/releases/tag/v4.0.0-nightly.20180821)|2018-08-21||yes|64|10.2.0|66.0.3359.181|432|
|[v4.0.0-nightly.20180819](https://github.com/electron/electron/releases/tag/v4.0.0-nightly.20180819)|2018-08-19||yes|64|10.2.0|66.0.3359.181|230|
|[v4.0.0-nightly.20180817](https://github.com/electron/electron/releases/tag/v4.0.0-nightly.20180817)|2018-08-18||yes|64|10.2.0|66.0.3359.181|179|
|[v4.0.0-beta.11](https://github.com/electron/electron/releases/tag/v4.0.0-beta.11)|2018-12-19|beta-4-0-x|yes|64|10.11.0|69.0.3497.106|5977|
|[v4.0.0-beta.10](https://github.com/electron/electron/releases/tag/v4.0.0-beta.10)|2018-12-17||yes|64|10.11.0|69.0.3497.106|5811|
|[v4.0.0-beta.9](https://github.com/electron/electron/releases/tag/v4.0.0-beta.9)|2018-12-11||yes|64|10.11.0|69.0.3497.106|4485|
|[v4.0.0-beta.8](https://github.com/electron/electron/releases/tag/v4.0.0-beta.8)|2018-11-30||yes|64|10.11.0|69.0.3497.106|4352|
|[v4.0.0-beta.7](https://github.com/electron/electron/releases/tag/v4.0.0-beta.7)|2018-11-06||yes|64|10.11.0|69.0.3497.106|9781|
|[v4.0.0-beta.6](https://github.com/electron/electron/releases/tag/v4.0.0-beta.6)|2018-11-01||yes|64|10.11.0|69.0.3497.106|5819|
|[v4.0.0-beta.5](https://github.com/electron/electron/releases/tag/v4.0.0-beta.5)|2018-10-23||yes|64|10.11.0|69.0.3497.106|7199|
|[v4.0.0-beta.4](https://github.com/electron/electron/releases/tag/v4.0.0-beta.4)|2018-10-19||yes|64|10.11.0|69.0.3497.106|5333|
|[v4.0.0-beta.3](https://github.com/electron/electron/releases/tag/v4.0.0-beta.3)|2018-10-12||yes|64|10.11.0|69.0.3497.106|5207|
|[v4.0.0-beta.2](https://github.com/electron/electron/releases/tag/v4.0.0-beta.2)|2018-10-12||yes|64|10.11.0|69.0.3497.106|3878|
|[v4.0.0-beta.1](https://github.com/electron/electron/releases/tag/v4.0.0-beta.1)|2018-10-11||yes|64|10.11.0|69.0.3497.106|26013|
|[v3.1.13](https://github.com/electron/electron/releases/tag/v3.1.13)|2019-07-31||no|64|10.2.0|66.0.3359.181|357327|
|[v3.1.12](https://github.com/electron/electron/releases/tag/v3.1.12)|2019-07-11|3-1-x|no|64|10.2.0|66.0.3359.181|46111|
|[v3.1.11](https://github.com/electron/electron/releases/tag/v3.1.11)|2019-06-06||no|64|10.2.0|66.0.3359.181|53844|
|[v3.1.10](https://github.com/electron/electron/releases/tag/v3.1.10)|2019-05-29||no|64|10.2.0|66.0.3359.181|46562|
|[v3.1.9](https://github.com/electron/electron/releases/tag/v3.1.9)|2019-05-01||no|64|10.2.0|66.0.3359.181|157808|
|[v3.1.8](https://github.com/electron/electron/releases/tag/v3.1.8)|2019-03-28||no|64|10.2.0|66.0.3359.181|154268|
|[v3.1.7](https://github.com/electron/electron/releases/tag/v3.1.7)|2019-03-21||no|64|10.2.0|66.0.3359.181|85068|
|[v3.1.6](https://github.com/electron/electron/releases/tag/v3.1.6)|2019-03-08||no|64|10.2.0|66.0.3359.181|118142|
|[v3.1.5](https://github.com/electron/electron/releases/tag/v3.1.5)|2019-03-04||no|64|10.2.0|66.0.3359.181|11967|
|[v3.1.4](https://github.com/electron/electron/releases/tag/v3.1.4)|2019-02-15||no|64|10.2.0|66.0.3359.181|41941|
|[v3.1.3](https://github.com/electron/electron/releases/tag/v3.1.3)|2019-01-31||no|64|10.2.0|66.0.3359.181|210842|
|[v3.1.2](https://github.com/electron/electron/releases/tag/v3.1.2)|2019-01-24||no|64|10.2.0|66.0.3359.181|32687|
|[v3.1.1](https://github.com/electron/electron/releases/tag/v3.1.1)|2019-01-14||no|64|10.2.0|66.0.3359.181|60229|
|[v3.1.0](https://github.com/electron/electron/releases/tag/v3.1.0)|2019-01-07||no|64|10.2.0|66.0.3359.181|53381|
|[v3.1.0-beta.5](https://github.com/electron/electron/releases/tag/v3.1.0-beta.5)|2019-01-04|beta-3-1-x|yes|64|10.2.0|66.0.3359.181|7110|
|[v3.1.0-beta.4](https://github.com/electron/electron/releases/tag/v3.1.0-beta.4)|2018-12-17||yes|64|10.2.0|66.0.3359.181|10054|
|[v3.1.0-beta.3](https://github.com/electron/electron/releases/tag/v3.1.0-beta.3)|2018-12-14||yes|64|10.2.0|66.0.3359.181|2849|
|[v3.1.0-beta.2](https://github.com/electron/electron/releases/tag/v3.1.0-beta.2)|2018-12-04||yes|64|10.2.0|66.0.3359.181|3575|
|[v3.1.0-beta.1](https://github.com/electron/electron/releases/tag/v3.1.0-beta.1)|2018-12-02||yes|64|10.2.0|66.0.3359.181|2007|
|[v3.0.16](https://github.com/electron/electron/releases/tag/v3.0.16)|2019-03-08|3-0-x|no|64|10.2.0|66.0.3359.181|33378|
|[v3.0.15](https://github.com/electron/electron/releases/tag/v3.0.15)|2019-01-31||no|64|10.2.0|66.0.3359.181|6869|
|[v3.0.14](https://github.com/electron/electron/releases/tag/v3.0.14)|2019-01-04||no|64|10.2.0|66.0.3359.181|37359|
|[v3.0.13](https://github.com/electron/electron/releases/tag/v3.0.13)|2018-12-17||no|64|10.2.0|66.0.3359.181|218362|
|[v3.0.12](https://github.com/electron/electron/releases/tag/v3.0.12)|2018-12-14||no|64|10.2.0|66.0.3359.181|29906|
|[v3.0.11](https://github.com/electron/electron/releases/tag/v3.0.11)|2018-12-11||no|64|10.2.0|66.0.3359.181|104798|
|[v3.0.10](https://github.com/electron/electron/releases/tag/v3.0.10)|2018-11-19||no|64|10.2.0|66.0.3359.181|348413|
|[v3.0.9](https://github.com/electron/electron/releases/tag/v3.0.9)|2018-11-13||no|64|10.2.0|66.0.3359.181|165399|
|[v3.0.8](https://github.com/electron/electron/releases/tag/v3.0.8)|2018-11-05||no|64|10.2.0|66.0.3359.181|123273|
|[v3.0.7](https://github.com/electron/electron/releases/tag/v3.0.7)|2018-11-01||no|64|10.2.0|66.0.3359.181|124604|
|[v3.0.6](https://github.com/electron/electron/releases/tag/v3.0.6)|2018-10-24||no|64|10.2.0|66.0.3359.181|105335|
|[v3.0.5](https://github.com/electron/electron/releases/tag/v3.0.5)|2018-10-19||no|64|10.2.0|66.0.3359.181|94690|
|[v3.0.4](https://github.com/electron/electron/releases/tag/v3.0.4)|2018-10-11||no|64|10.2.0|66.0.3359.181|118456|
|[v3.0.3](https://github.com/electron/electron/releases/tag/v3.0.3)|2018-10-07||no|64|10.2.0|66.0.3359.181|75342|
|[v3.0.2](https://github.com/electron/electron/releases/tag/v3.0.2)|2018-09-27||no|64|10.2.0|66.0.3359.181|114859|
|[v3.0.1](https://github.com/electron/electron/releases/tag/v3.0.1)|2018-09-27||no|64|10.2.0|66.0.3359.181|16019|
|[v3.0.0](https://github.com/electron/electron/releases/tag/v3.0.0)|2018-09-18||no|64|10.2.0|66.0.3359.181|1320322|
|[v3.0.0-nightly.20180904](https://github.com/electron/electron/releases/tag/v3.0.0-nightly.20180904)|2018-09-05||yes|64|10.2.0|66.0.3359.181|1071|
|[v3.0.0-nightly.20180823](https://github.com/electron/electron/releases/tag/v3.0.0-nightly.20180823)|2018-08-24||yes|64|10.2.0|66.0.3359.181|260|
|[v3.0.0-nightly.20180821](https://github.com/electron/electron/releases/tag/v3.0.0-nightly.20180821)|2018-08-21||yes|64|10.2.0|66.0.3359.181|994|
|[v3.0.0-nightly.20180818](https://github.com/electron/electron/releases/tag/v3.0.0-nightly.20180818)|2018-08-18||yes|64|10.2.0|66.0.3359.181|374|
|[v3.0.0-beta.13](https://github.com/electron/electron/releases/tag/v3.0.0-beta.13)|2018-09-17||yes|64|10.2.0|66.0.3359.181|3385|
|[v3.0.0-beta.12](https://github.com/electron/electron/releases/tag/v3.0.0-beta.12)|2018-09-12||yes|64|10.2.0|66.0.3359.181|7932|
|[v3.0.0-beta.11](https://github.com/electron/electron/releases/tag/v3.0.0-beta.11)|2018-09-12||yes|64|10.2.0|66.0.3359.181|3087|
|[v3.0.0-beta.10](https://github.com/electron/electron/releases/tag/v3.0.0-beta.10)|2018-09-08||yes|64|10.2.0|66.0.3359.181|4385|
|[v3.0.0-beta.9](https://github.com/electron/electron/releases/tag/v3.0.0-beta.9)|2018-09-06||yes|64|10.2.0|66.0.3359.181|3728|
|[v3.0.0-beta.8](https://github.com/electron/electron/releases/tag/v3.0.0-beta.8)|2018-08-29||yes|64|10.2.0|66.0.3359.181|6894|
|[v3.0.0-beta.7](https://github.com/electron/electron/releases/tag/v3.0.0-beta.7)|2018-08-22||yes|64|10.2.0|66.0.3359.181|5181|
|[v3.0.0-beta.6](https://github.com/electron/electron/releases/tag/v3.0.0-beta.6)|2018-08-20||yes|64|10.2.0|66.0.3359.181|4981|
|[v3.0.0-beta.5](https://github.com/electron/electron/releases/tag/v3.0.0-beta.5)|2018-08-13||yes|64|10.2.0|66.0.3359.181|20595|
|[v3.0.0-beta.4](https://github.com/electron/electron/releases/tag/v3.0.0-beta.4)|2018-08-01||yes|64|10.2.0|66.0.3359.181|16521|
|[v3.0.0-beta.3](https://github.com/electron/electron/releases/tag/v3.0.0-beta.3)|2018-07-17||yes|64|10.2.0|66.0.3359.181|7771|
|[v3.0.0-beta.2](https://github.com/electron/electron/releases/tag/v3.0.0-beta.2)|2018-07-10||yes|64|10.2.0|66.0.3359.181|6022|
|[v3.0.0-beta.1](https://github.com/electron/electron/releases/tag/v3.0.0-beta.1)|2018-06-21||yes|64|10.2.0|66.0.3359.181|35220|
|[v2.1.0-unsupported.20180809](https://github.com/electron/electron/releases/tag/v2.1.0-unsupported.20180809)|2018-08-09|unsupported|no|57|8.9.3|61.0.3163.100|8592|
|[v2.0.18](https://github.com/electron/electron/releases/tag/v2.0.18)|2019-03-08|2-0-x|no|57|8.9.3|61.0.3163.100|1484807|
|[v2.0.17](https://github.com/electron/electron/releases/tag/v2.0.17)|2019-01-31||no|57|8.9.3|61.0.3163.100|105277|
|[v2.0.16](https://github.com/electron/electron/releases/tag/v2.0.16)|2018-12-18||no|57|8.9.3|61.0.3163.100|199821|
|[v2.0.15](https://github.com/electron/electron/releases/tag/v2.0.15)|2018-12-13||no|57|8.9.3|61.0.3163.100|15272|
|[v2.0.14](https://github.com/electron/electron/releases/tag/v2.0.14)|2018-11-19||no|57|8.9.3|61.0.3163.100|120533|
|[v2.0.13](https://github.com/electron/electron/releases/tag/v2.0.13)|2018-11-01||no|57|8.9.3|61.0.3163.100|112724|
|[v2.0.12](https://github.com/electron/electron/releases/tag/v2.0.12)|2018-10-19||no|57|8.9.3|61.0.3163.100|147178|
|[v2.0.11](https://github.com/electron/electron/releases/tag/v2.0.11)|2018-10-01||no|57|8.9.3|61.0.3163.100|106536|
|[v2.0.10](https://github.com/electron/electron/releases/tag/v2.0.10)|2018-09-19||no|57|8.9.3|61.0.3163.100|100137|
|[v2.0.9](https://github.com/electron/electron/releases/tag/v2.0.9)|2018-09-10||no|57|8.9.3|61.0.3163.100|301943|
|[v2.0.8](https://github.com/electron/electron/releases/tag/v2.0.8)|2018-08-22||no|57|8.9.3|61.0.3163.100|918566|
|[v2.0.8-nightly.20180820](https://github.com/electron/electron/releases/tag/v2.0.8-nightly.20180820)|2018-08-21||yes|57|8.9.3|61.0.3163.100|235|
|[v2.0.8-nightly.20180819](https://github.com/electron/electron/releases/tag/v2.0.8-nightly.20180819)|2018-08-20||yes|57|8.9.3|61.0.3163.100|220|
|[v2.0.7](https://github.com/electron/electron/releases/tag/v2.0.7)|2018-08-08||no|57|8.9.3|61.0.3163.100|226218|
|[v2.0.6](https://github.com/electron/electron/releases/tag/v2.0.6)|2018-08-01||no|57|8.9.3|61.0.3163.100|192426|
|[v2.0.5](https://github.com/electron/electron/releases/tag/v2.0.5)|2018-07-13||no|57|8.9.3|61.0.3163.100|269655|
|[v2.0.4](https://github.com/electron/electron/releases/tag/v2.0.4)|2018-07-03||no|57|8.9.3|61.0.3163.100|173129|
|[v2.0.3](https://github.com/electron/electron/releases/tag/v2.0.3)|2018-06-21||no|57|8.9.3|61.0.3163.100|144034|
|[v2.0.2](https://github.com/electron/electron/releases/tag/v2.0.2)|2018-05-22||no|57|8.9.3|61.0.3163.100|474453|
|[v2.0.1](https://github.com/electron/electron/releases/tag/v2.0.1)|2018-05-16||no|57|8.9.3|61.0.3163.100|127367|
|[v2.0.0](https://github.com/electron/electron/releases/tag/v2.0.0)|2018-05-01||no|57|8.9.3|61.0.3163.100|721131|
|[v2.0.0-beta.8](https://github.com/electron/electron/releases/tag/v2.0.0-beta.8)|2018-04-26||yes|57|8.9.3|61.0.3163.100|19408|
|[v2.0.0-beta.7](https://github.com/electron/electron/releases/tag/v2.0.0-beta.7)|2018-04-03||yes|57|8.9.3|61.0.3163.100|43673|
|[v2.0.0-beta.6](https://github.com/electron/electron/releases/tag/v2.0.0-beta.6)|2018-03-27||yes|57|8.9.3|61.0.3163.100|15406|
|[v2.0.0-beta.5](https://github.com/electron/electron/releases/tag/v2.0.0-beta.5)|2018-03-20||yes|57|8.9.3|61.0.3163.100|14201|
|[v2.0.0-beta.4](https://github.com/electron/electron/releases/tag/v2.0.0-beta.4)|2018-03-15||yes|57|8.9.3|61.0.3163.100|12982|
|[v2.0.0-beta.3](https://github.com/electron/electron/releases/tag/v2.0.0-beta.3)|2018-03-09||yes|57|8.9.3|61.0.3163.100|11388|
|[v2.0.0-beta.2](https://github.com/electron/electron/releases/tag/v2.0.0-beta.2)|2018-03-05||yes|57|8.9.3|61.0.3163.100|20622|
|[v2.0.0-beta.1](https://github.com/electron/electron/releases/tag/v2.0.0-beta.1)|2018-02-21||yes|57|8.9.3|61.0.3163.100|15131|
|[v1.8.8](https://github.com/electron/electron/releases/tag/v1.8.8)|2018-08-22|1-8-x|no|57|8.2.1|59.0.3071.115|4105182|
|[v1.8.7](https://github.com/electron/electron/releases/tag/v1.8.7)|2018-05-16||no|57|8.2.1|59.0.3071.115|1053898|
|[v1.8.6](https://github.com/electron/electron/releases/tag/v1.8.6)|2018-04-27||no|57|8.2.1|59.0.3071.115|276779|
|[v1.8.5](https://github.com/electron/electron/releases/tag/v1.8.5)|2018-04-26||no|57|8.2.1|59.0.3071.115|22030|
|[v1.8.4](https://github.com/electron/electron/releases/tag/v1.8.4)|2018-03-16||no|57|8.2.1|59.0.3071.115|920810|
|[v1.8.3](https://github.com/electron/electron/releases/tag/v1.8.3)|2018-03-06||no|57|8.2.1|59.0.3071.115|408022|
|[v1.8.2](https://github.com/electron/electron/releases/tag/v1.8.2)|2018-02-07||no|57|8.2.1|59.0.3071.115|606895|
|[v1.8.2-beta.5](https://github.com/electron/electron/releases/tag/v1.8.2-beta.5)|2018-01-31||yes|57|8.2.1|59.0.3071.115|152438|
|[v1.8.2-beta.4](https://github.com/electron/electron/releases/tag/v1.8.2-beta.4)|2018-01-23||yes|57|8.2.1|59.0.3071.115|22912|
|[v1.8.2-beta.3](https://github.com/electron/electron/releases/tag/v1.8.2-beta.3)|2017-12-04||yes|57|8.2.1|59.0.3071.115|39031|
|[v1.8.2-beta.2](https://github.com/electron/electron/releases/tag/v1.8.2-beta.2)|2017-11-06||yes|57|8.2.1|59.0.3071.115|33371|
|[v1.8.2-beta.1](https://github.com/electron/electron/releases/tag/v1.8.2-beta.1)|2017-10-19||yes|57|8.2.1|59.0.3071.115|20268|
|[v1.8.1](https://github.com/electron/electron/releases/tag/v1.8.1)|2017-09-29||yes|57|8.2.1|59.0.3071.115|363941|
|[v1.8.0](https://github.com/electron/electron/releases/tag/v1.8.0)|2017-12-12||yes|57|8.2.1|59.0.3071.115|1640508|
|[v1.7.16](https://github.com/electron/electron/releases/tag/v1.7.16)|2018-08-22|1-7-x|no|54|7.9.0|58.0.3029.110|214652|
|[v1.7.15](https://github.com/electron/electron/releases/tag/v1.7.15)|2018-05-16||no|54|7.9.0|58.0.3029.110|33960|
|[v1.7.14](https://github.com/electron/electron/releases/tag/v1.7.14)|2018-04-27||no|54|7.9.0|58.0.3029.110|19255|
|[v1.7.13](https://github.com/electron/electron/releases/tag/v1.7.13)|2018-03-15||no|54|7.9.0|58.0.3029.110|83414|
|[v1.7.12](https://github.com/electron/electron/releases/tag/v1.7.12)|2018-01-31||no|54|7.9.0|58.0.3029.110|310173|
|[v1.7.11](https://github.com/electron/electron/releases/tag/v1.7.11)|2018-01-23||no|54|7.9.0|58.0.3029.110|465946|
|[v1.7.10](https://github.com/electron/electron/releases/tag/v1.7.10)|2017-12-18||no|54|7.9.0|58.0.3029.110|868677|
|[v1.7.9](https://github.com/electron/electron/releases/tag/v1.7.9)|2017-10-11||no|54|7.9.0|58.0.3029.110|1319895|
|[v1.7.8](https://github.com/electron/electron/releases/tag/v1.7.8)|2017-09-24||no|54|7.9.0|58.0.3029.110|365199|
|[v1.7.7](https://github.com/electron/electron/releases/tag/v1.7.7)|2017-09-05||yes|54|7.9.0|58.0.3029.110|150182|
|[v1.7.6](https://github.com/electron/electron/releases/tag/v1.7.6)|2017-08-09||no|54|7.9.0|58.0.3029.110|431350|
|[v1.7.5](https://github.com/electron/electron/releases/tag/v1.7.5)|2017-07-17||no|54|7.9.0|58.0.3029.110|436253|
|[v1.7.4](https://github.com/electron/electron/releases/tag/v1.7.4)|2017-06-28||yes|54|7.9.0|58.0.3029.110|80658|
|[v1.7.3](https://github.com/electron/electron/releases/tag/v1.7.3)|2017-06-08||yes|54|7.9.0|58.0.3029.110|93026|
|[v1.7.2](https://github.com/electron/electron/releases/tag/v1.7.2)|2017-05-26||yes|54|7.9.0|58.0.3029.110|50926|
|[v1.7.1](https://github.com/electron/electron/releases/tag/v1.7.1)|2017-05-16||yes|54|7.9.0|58.0.3029.110|221654|
|[v1.7.0](https://github.com/electron/electron/releases/tag/v1.7.0)|2017-05-10||yes|54|7.9.0|58.0.3029.110|819929|
|[v1.6.18](https://github.com/electron/electron/releases/tag/v1.6.18)|2018-05-15||no|53|7.4.0|56.0.2924.87|49648|
|[v1.6.17](https://github.com/electron/electron/releases/tag/v1.6.17)|2018-01-31||no|53|7.4.0|56.0.2924.87|25551|
|[v1.6.16](https://github.com/electron/electron/releases/tag/v1.6.16)|2018-01-23||no|53|7.4.0|56.0.2924.87|29803|
|[v1.6.15](https://github.com/electron/electron/releases/tag/v1.6.15)|2017-10-11||no|53|7.4.0|56.0.2924.87|80897|
|[v1.6.14](https://github.com/electron/electron/releases/tag/v1.6.14)|2017-09-28||no|53|7.4.0|56.0.2924.87|29843|
|[v1.6.13](https://github.com/electron/electron/releases/tag/v1.6.13)|2017-09-06||yes|53|7.4.0|56.0.2924.87|44969|
|[v1.6.12](https://github.com/electron/electron/releases/tag/v1.6.12)|2017-09-06||yes|53|7.4.0|56.0.2924.87|49483|
|[v1.6.11](https://github.com/electron/electron/releases/tag/v1.6.11)|2017-05-25||no|53|7.4.0|56.0.2924.87|782437|
|[v1.6.10](https://github.com/electron/electron/releases/tag/v1.6.10)|2017-05-16||no|53|7.4.0|56.0.2924.87|271662|
|[v1.6.9](https://github.com/electron/electron/releases/tag/v1.6.9)|2017-05-10||no|53|7.4.0|56.0.2924.87|18589|
|[v1.6.8](https://github.com/electron/electron/releases/tag/v1.6.8)|2017-05-01||no|53|7.4.0|56.0.2924.87|153973|
|[v1.6.7](https://github.com/electron/electron/releases/tag/v1.6.7)|2017-04-18||no|53|7.4.0|56.0.2924.87|148979|
|[v1.6.6](https://github.com/electron/electron/releases/tag/v1.6.6)|2017-04-07||no|53|7.4.0|56.0.2924.87|340176|
|[v1.6.5](https://github.com/electron/electron/releases/tag/v1.6.5)|2017-03-31||no|53|7.4.0|56.0.2924.87|156843|
|[v1.6.4](https://github.com/electron/electron/releases/tag/v1.6.4)|2017-03-22||yes|53|7.4.0|56.0.2924.87|36318|
|[v1.6.3](https://github.com/electron/electron/releases/tag/v1.6.3)|2017-03-07||yes|53|7.4.0|56.0.2924.87|57334|
|[v1.6.2](https://github.com/electron/electron/releases/tag/v1.6.2)|2017-03-01||no|53|7.4.0|56.0.2924.87|614297|
|[v1.6.1](https://github.com/electron/electron/releases/tag/v1.6.1)|2017-02-21||no|53|7.4.0|56.0.2924.87|221128|
|[v1.6.0](https://github.com/electron/electron/releases/tag/v1.6.0)|2017-02-07||yes|53|7.4.0|56.0.2924.87|405208|
|[v1.5.1](https://github.com/electron/electron/releases/tag/v1.5.1)|2017-02-06||yes|51|7.4.0|54.0.2840.101|18891|
|[v1.5.0](https://github.com/electron/electron/releases/tag/v1.5.0)|2017-01-24||yes|51|7.4.0|54.0.2840.101|207556|
|[v1.4.16](https://github.com/electron/electron/releases/tag/v1.4.16)|2017-04-05||no|50|6.5.0|53.0.2785.143|242446|
|[v1.4.15](https://github.com/electron/electron/releases/tag/v1.4.15)|2017-01-19||no|50|6.5.0|53.0.2785.143|1197593|
|[v1.4.14](https://github.com/electron/electron/releases/tag/v1.4.14)|2017-01-10||no|50|6.5.0|53.0.2785.143|182642|
|[v1.4.13](https://github.com/electron/electron/releases/tag/v1.4.13)|2016-12-20||no|50|6.5.0|53.0.2785.143|1102545|
|[v1.4.12](https://github.com/electron/electron/releases/tag/v1.4.12)|2016-12-10||no|50|6.5.0|54.0.2840.51|120753|
|[v1.4.11](https://github.com/electron/electron/releases/tag/v1.4.11)|2016-12-07||no|50|6.5.0|53.0.2785.143|27169|
|[v1.4.10](https://github.com/electron/electron/releases/tag/v1.4.10)|2016-11-28||no|50|6.5.0|53.0.2785.143|96594|
|[v1.4.8](https://github.com/electron/electron/releases/tag/v1.4.8)|2016-11-22||no|50|6.5.0|53.0.2785.143|46326|
|[v1.4.7](https://github.com/electron/electron/releases/tag/v1.4.7)|2016-11-16||no|50|6.5.0|53.0.2785.143|49100|
|[v1.4.6](https://github.com/electron/electron/releases/tag/v1.4.6)|2016-11-09||no|50|6.5.0|53.0.2785.143|124212|
|[v1.4.5](https://github.com/electron/electron/releases/tag/v1.4.5)|2016-11-01||no|50|6.5.0|53.0.2785.113|62482|
|[v1.4.4](https://github.com/electron/electron/releases/tag/v1.4.4)|2016-10-20||no|50|6.5.0|53.0.2785.113|99484|
|[v1.4.3](https://github.com/electron/electron/releases/tag/v1.4.3)|2016-10-06||no|50|6.5.0|53.0.2785.113|138192|
|[v1.4.2](https://github.com/electron/electron/releases/tag/v1.4.2)|2016-09-30||no|50|6.5.0|53.0.2785.113|41021|
|[v1.4.1](https://github.com/electron/electron/releases/tag/v1.4.1)|2016-09-22||no|50|6.5.0|53.0.2785.113|62313|
|[v1.4.0](https://github.com/electron/electron/releases/tag/v1.4.0)|2016-09-15||no|50|6.5.0|53.0.2785.113|244160|
|[v1.3.15](https://github.com/electron/electron/releases/tag/v1.3.15)|2017-04-21||no|49|6.5.0|52.0.2743.82|18942|
|[v1.3.14](https://github.com/electron/electron/releases/tag/v1.3.14)|2017-03-14||no|49|6.5.0|52.0.2743.82|20572|
|[v1.3.13](https://github.com/electron/electron/releases/tag/v1.3.13)|2016-12-06||no|49|6.5.0|52.0.2743.82|28876|
|[v1.3.12](https://github.com/electron/electron/releases/tag/v1.3.12)|2016-11-28||no||||2029|
|[v1.3.10](https://github.com/electron/electron/releases/tag/v1.3.10)|2016-11-22||no|49|6.5.0|52.0.2743.82|1851|
|[v1.3.9](https://github.com/electron/electron/releases/tag/v1.3.9)|2016-11-16||no|49|6.5.0|52.0.2743.82|54613|
|[v1.3.8](https://github.com/electron/electron/releases/tag/v1.3.8)|2016-10-20||no||||35774|
|[v1.3.7](https://github.com/electron/electron/releases/tag/v1.3.7)|2016-09-27||no|49|6.5.0|52.0.2743.82|13046|
|[v1.3.6](https://github.com/electron/electron/releases/tag/v1.3.6)|2016-09-15||no|49|6.3.0|52.0.2743.82|38933|
|[v1.3.5](https://github.com/electron/electron/releases/tag/v1.3.5)|2016-09-02||no|49|6.3.0|52.0.2743.82|92432|
|[v1.3.4](https://github.com/electron/electron/releases/tag/v1.3.4)|2016-08-23||no|49|6.3.0|52.0.2743.82|109557|
|[v1.3.3](https://github.com/electron/electron/releases/tag/v1.3.3)|2016-08-10||no|49|6.3.0|52.0.2743.82|135119|
|[v1.3.2](https://github.com/electron/electron/releases/tag/v1.3.2)|2016-08-02||no|49|6.3.0|52.0.2743.82|75893|
|[v1.3.1](https://github.com/electron/electron/releases/tag/v1.3.1)|2016-07-27||no|49|6.3.0|52.0.2743.82|134829|
|[v1.3.0](https://github.com/electron/electron/releases/tag/v1.3.0)|2016-07-25||no|49|6.3.0|52.0.2743.82|133334|
|[v1.2.8](https://github.com/electron/electron/releases/tag/v1.2.8)|2016-07-21||no|48|6.1.0|51.0.2704.106|111021|
|[v1.2.7](https://github.com/electron/electron/releases/tag/v1.2.7)|2016-07-13||no|48|6.1.0|51.0.2704.106|69488|
|[v1.2.6](https://github.com/electron/electron/releases/tag/v1.2.6)|2016-07-06||no|48|6.1.0|51.0.2704.106|75544|
|[v1.2.5](https://github.com/electron/electron/releases/tag/v1.2.5)|2016-06-23||no|48|6.1.0|51.0.2704.103|77900|
|[v1.2.4](https://github.com/electron/electron/releases/tag/v1.2.4)|2016-06-22||no|48|6.1.0|51.0.2704.103|30401|
|[v1.2.3](https://github.com/electron/electron/releases/tag/v1.2.3)|2016-06-16||no|48|6.1.0|51.0.2704.84|52530|
|[v1.2.2](https://github.com/electron/electron/releases/tag/v1.2.2)|2016-06-08||no|48|6.1.0|51.0.2704.84|156471|
|[v1.2.1](https://github.com/electron/electron/releases/tag/v1.2.1)|2016-06-01||no|48|6.1.0|51.0.2704.63|64474|
|[v1.2.0](https://github.com/electron/electron/releases/tag/v1.2.0)|2016-05-26||no|48|6.1.0|51.0.2704.63|118315|
|[v1.1.3](https://github.com/electron/electron/releases/tag/v1.1.3)|2016-05-25||no|48|6.1.0|50.0.2661.102|76009|
|[v1.1.2](https://github.com/electron/electron/releases/tag/v1.1.2)|2016-05-24||no|48|6.1.0|50.0.2661.102|31408|
|[v1.1.1](https://github.com/electron/electron/releases/tag/v1.1.1)|2016-05-20||no|48|6.1.0|50.0.2661.102|74603|
|[v1.1.0](https://github.com/electron/electron/releases/tag/v1.1.0)|2016-05-14||no|48|6.1.0|50.0.2661.102|83304|
|[v1.0.2](https://github.com/electron/electron/releases/tag/v1.0.2)|2016-05-13||no|47|5.10.0|49.0.2623.75|49777|
|[v1.0.1](https://github.com/electron/electron/releases/tag/v1.0.1)|2016-05-11||no|47|5.10.0|49.0.2623.75|44143|
|[v1.0.0](https://github.com/electron/electron/releases/tag/v1.0.0)|2016-05-11||no|47|5.10.0|49.0.2623.75|153219|
|[v0.37.8](https://github.com/electron/electron/releases/tag/v0.37.8)|2016-04-29||no|47|5.10.0|49.0.2623.75|252800|
|[v0.37.7](https://github.com/electron/electron/releases/tag/v0.37.7)|2016-04-22||no|47|5.10.0|49.0.2623.75|58626|
|[v0.37.6](https://github.com/electron/electron/releases/tag/v0.37.6)|2016-04-15||no|47|5.10.0|49.0.2623.75|98017|
|[v0.37.5](https://github.com/electron/electron/releases/tag/v0.37.5)|2016-04-07||no|47|5.10.0|49.0.2623.75|58115|
|[v0.37.4](https://github.com/electron/electron/releases/tag/v0.37.4)|2016-04-03||no|47|6.0.0-pre|49.0.2623.75|61030|
|[v0.37.3](https://github.com/electron/electron/releases/tag/v0.37.3)|2016-03-27||no|47|5.1.1|49.0.2623.75|177734|
|[v0.37.2](https://github.com/electron/electron/releases/tag/v0.37.2)|2016-03-14||no||||76530|
|[v0.37.1](https://github.com/electron/electron/releases/tag/v0.37.1)|2016-03-13||no|47|5.1.1|49.0.2623.75|24364|
|[v0.37.0](https://github.com/electron/electron/releases/tag/v0.37.0)|2016-03-12||no|47|5.1.1|49.0.2623.75|123101|
|[v0.36.12](https://github.com/electron/electron/releases/tag/v0.36.12)|2016-03-27||no|47|5.1.1|47.0.2526.110|219177|
|[v0.36.11](https://github.com/electron/electron/releases/tag/v0.36.11)|2016-03-11||no|47|5.1.1|47.0.2526.110|47063|
|[v0.36.10](https://github.com/electron/electron/releases/tag/v0.36.10)|2016-03-05||no|47|5.1.1|47.0.2526.110|47581|
|[v0.36.9](https://github.com/electron/electron/releases/tag/v0.36.9)|2016-02-26||no|47|5.1.1|47.0.2526.110|62843|
|[v0.36.8](https://github.com/electron/electron/releases/tag/v0.36.8)|2016-02-19||no|47|5.1.1|47.0.2526.110|54058|
|[v0.36.7](https://github.com/electron/electron/releases/tag/v0.36.7)|2016-01-30||no|47|5.1.1|47.0.2526.110|131810|
|[v0.36.6](https://github.com/electron/electron/releases/tag/v0.36.6)|2016-01-29||no|47|5.1.1|47.0.2526.110|20246|
|[v0.36.5](https://github.com/electron/electron/releases/tag/v0.36.5)|2016-01-22||no|47|5.1.1|47.0.2526.110|42513|
|[v0.36.4](https://github.com/electron/electron/releases/tag/v0.36.4)|2016-01-15||no|47|5.1.1|47.0.2526.73|63604|
|[v0.36.3](https://github.com/electron/electron/releases/tag/v0.36.3)|2016-01-11||no|47|5.1.1|47.0.2526.73|37122|
|[v0.36.2](https://github.com/electron/electron/releases/tag/v0.36.2)|2015-12-25||no|47|5.1.1|47.0.2526.73|59763|
|[v0.36.1](https://github.com/electron/electron/releases/tag/v0.36.1)|2015-12-18||no||||34122|
|[v0.36.0](https://github.com/electron/electron/releases/tag/v0.36.0)|2015-12-11||no|47|5.1.1|47.0.2526.73|107084|
|[v0.35.6](https://github.com/electron/electron/releases/tag/v0.35.6)|2016-01-11||no||||123219|
|[v0.35.5](https://github.com/electron/electron/releases/tag/v0.35.5)|2015-12-31||no|46|4.1.1|45.0.2454.85|18011|
|[v0.35.4](https://github.com/electron/electron/releases/tag/v0.35.4)|2015-12-04||no|46|4.1.1|45.0.2454.85|89110|
|[v0.35.3](https://github.com/electron/electron/releases/tag/v0.35.3)|2015-12-04||no|46|4.1.1|45.0.2454.85|55269|
|[v0.35.2](https://github.com/electron/electron/releases/tag/v0.35.2)|2015-11-27||no|46|4.1.1|45.0.2454.85|38171|
|[v0.35.1](https://github.com/electron/electron/releases/tag/v0.35.1)|2015-11-20||no|46|4.1.1|45.0.2454.85|36459|
|[v0.35.0](https://github.com/electron/electron/releases/tag/v0.35.0)|2015-11-16||no||||44886|
|[v0.34.5](https://github.com/electron/electron/releases/tag/v0.34.5)|2015-11-26||no||||64071|
|[v0.34.4](https://github.com/electron/electron/releases/tag/v0.34.4)|2015-11-24||no|46|4.1.1|45.0.2454.85|14774|
|[v0.34.3](https://github.com/electron/electron/releases/tag/v0.34.3)|2015-11-06||no|46|4.1.1|45.0.2454.85|52113|
|[v0.34.2](https://github.com/electron/electron/releases/tag/v0.34.2)|2015-10-30||no|46|4.1.1|45.0.2454.85|37583|
|[v0.34.1](https://github.com/electron/electron/releases/tag/v0.34.1)|2015-10-23||no|46|4.1.1|45.0.2454.85|38939|
|[v0.34.0](https://github.com/electron/electron/releases/tag/v0.34.0)|2015-10-16||no|46|4.1.1|45.0.2454.85|118071|
|[v0.33.9](https://github.com/electron/electron/releases/tag/v0.33.9)|2015-10-16||no|46|4.1.1|45.0.2454.85|50781|
|[v0.33.8](https://github.com/electron/electron/releases/tag/v0.33.8)|2015-10-14||no|46|4.1.1|45.0.2454.85|15343|
|[v0.33.7](https://github.com/electron/electron/releases/tag/v0.33.7)|2015-10-10||no|46|4.1.1|45.0.2454.85|21725|
|[v0.33.6](https://github.com/electron/electron/releases/tag/v0.33.6)|2015-10-05||no|46|4.1.1|45.0.2454.85|23312|
|[v0.33.5](https://github.com/electron/electron/releases/tag/v0.33.5)|2015-10-05||no||||12315|
|[v0.33.4](https://github.com/electron/electron/releases/tag/v0.33.4)|2015-10-02||no|46|4.1.1|45.0.2454.85|17128|
|[v0.33.3](https://github.com/electron/electron/releases/tag/v0.33.3)|2015-09-26||no|46|4.1.1|45.0.2454.85|30941|
|[v0.33.2](https://github.com/electron/electron/releases/tag/v0.33.2)|2015-09-25||no|46|4.1.1|45.0.2454.85|13008|
|[v0.33.1](https://github.com/electron/electron/releases/tag/v0.33.1)|2015-09-22||no|46|4.1.1|45.0.2454.85|16344|
|[v0.33.0](https://github.com/electron/electron/releases/tag/v0.33.0)|2015-09-17||no|46|5.0.0-pre|45.0.2454.85|31399|
|[v0.32.3](https://github.com/electron/electron/releases/tag/v0.32.3)|2015-09-15||no|46|5.0.0-pre|45.0.2454.85|20543|
|[v0.32.2](https://github.com/electron/electron/releases/tag/v0.32.2)|2015-09-10||no|45|3.3.0|45.0.2454.85|14918|
|[v0.32.1](https://github.com/electron/electron/releases/tag/v0.32.1)|2015-09-09||no||||18097|
|[v0.32.0](https://github.com/electron/electron/releases/tag/v0.32.0)|2015-09-09||no||||1387|
|[v0.31.2](https://github.com/electron/electron/releases/tag/v0.31.2)|2015-09-02||no|45|3.3.0|45.0.2454.85|25292|
|[v0.31.1](https://github.com/electron/electron/releases/tag/v0.31.1)|2015-08-28||no||||8998|
|[v0.31.0](https://github.com/electron/electron/releases/tag/v0.31.0)|2015-08-26||no|45|3.1.0|44.0.2403.125|6231|
|[v0.30.8](https://github.com/electron/electron/releases/tag/v0.30.8)|2015-09-26||no||||150391|
|[v0.30.7](https://github.com/electron/electron/releases/tag/v0.30.7)|2015-09-24||no||||23411|
|[v0.30.6](https://github.com/electron/electron/releases/tag/v0.30.6)|2015-08-26||no||||10853|
|[v0.30.5](https://github.com/electron/electron/releases/tag/v0.30.5)|2015-08-21||no||||4018|
|[v0.30.4](https://github.com/electron/electron/releases/tag/v0.30.4)|2015-08-10||no|45|3.1.0|44.0.2403.125|15230|
|[v0.30.3](https://github.com/electron/electron/releases/tag/v0.30.3)|2015-08-07||no||||6305|
|[v0.30.2](https://github.com/electron/electron/releases/tag/v0.30.2)|2015-07-30||no||||24914|
|[v0.30.1](https://github.com/electron/electron/releases/tag/v0.30.1)|2015-07-24||no||||13610|
|[v0.30.0](https://github.com/electron/electron/releases/tag/v0.30.0)|2015-07-16||no||||30088|
|[v0.29.2](https://github.com/electron/electron/releases/tag/v0.29.2)|2015-07-07||no|44|2.3.1|43.0.2357.65|56907|
|[v0.29.1](https://github.com/electron/electron/releases/tag/v0.29.1)|2015-07-03||no|44|2.3.1|43.0.2357.65|3863|
|[v0.29.0](https://github.com/electron/electron/releases/tag/v0.29.0)|2015-07-03||no||||2162|
|[v0.28.3](https://github.com/electron/electron/releases/tag/v0.28.3)|2015-06-23||no|44|2.2.1|43.0.2357.65|25279|
|[v0.28.2](https://github.com/electron/electron/releases/tag/v0.28.2)|2015-06-18||no|44|2.2.1|43.0.2357.65|7176|
|[v0.28.1](https://github.com/electron/electron/releases/tag/v0.28.1)|2015-06-12||no|44|2.2.1|43.0.2357.65|8255|
|[v0.28.0](https://github.com/electron/electron/releases/tag/v0.28.0)|2015-06-11||no|44|2.2.1|43.0.2357.65|1184|
|[v0.27.3](https://github.com/electron/electron/releases/tag/v0.27.3)|2015-06-08||no|43|1.6.3|43.0.2357.65|13696|
|[v0.27.2](https://github.com/electron/electron/releases/tag/v0.27.2)|2015-06-01||no|43|1.6.3|43.0.2357.65|6622|
|[v0.27.1](https://github.com/electron/electron/releases/tag/v0.27.1)|2015-05-28||no|43|1.6.3|42.0.2311.107|3054|
|[v0.27.0](https://github.com/electron/electron/releases/tag/v0.27.0)|2015-05-27||no|43|1.6.3|42.0.2311.107|1381|
|[v0.26.1](https://github.com/electron/electron/releases/tag/v0.26.1)|2015-05-21||no|43|1.6.3|42.0.2311.107|12100|
|[v0.26.0](https://github.com/electron/electron/releases/tag/v0.26.0)|2015-05-12||no|43|1.6.3|42.0.2311.107|10859|
|[v0.25.3](https://github.com/electron/electron/releases/tag/v0.25.3)|2015-05-08||no|43|1.6.3|42.0.2311.107|15939|
|[v0.25.2](https://github.com/electron/electron/releases/tag/v0.25.2)|2015-05-01||no|43|1.6.3|42.0.2311.107|8353|
|[v0.25.1](https://github.com/electron/electron/releases/tag/v0.25.1)|2015-04-23||no|43|1.6.3|42.0.2311.107|15666|
|[v0.25.0](https://github.com/electron/electron/releases/tag/v0.25.0)|2015-04-22||no|43|1.6.3|42.0.2311.107|1621|
|[v0.24.0](https://github.com/electron/electron/releases/tag/v0.24.0)|2015-04-17||no|43|1.6.3|41.0.2272.76|8112|
|[v0.23.0](https://github.com/electron/electron/releases/tag/v0.23.0)|2015-04-12||no|43|1.6.3|41.0.2272.76|3013|
|[v0.22.3](https://github.com/electron/electron/releases/tag/v0.22.3)|2015-03-30||no|43|1.6.3|41.0.2272.76|40786|
|[v0.22.2](https://github.com/electron/electron/releases/tag/v0.22.2)|2015-03-23||no|43|1.5.1|41.0.2272.76|1596|
|[v0.22.1](https://github.com/electron/electron/releases/tag/v0.22.1)|2015-03-18||no|43|1.5.1|41.0.2272.76|4217|
|[v0.22.0](https://github.com/electron/electron/releases/tag/v0.22.0)|2015-03-18||no||||33495|
|[v0.21.3](https://github.com/electron/electron/releases/tag/v0.21.3)|2015-03-03||no|43|1.5.1|41.0.2272.76|8344|
|[v0.21.2](https://github.com/electron/electron/releases/tag/v0.21.2)|2015-02-05||no|41|1.0.0-pre|40.0.2214.91|4422|
|[v0.21.1](https://github.com/electron/electron/releases/tag/v0.21.1)|2015-02-03||no|41|1.0.0-pre|40.0.2214.91|1588|
|[v0.21.0](https://github.com/electron/electron/releases/tag/v0.21.0)|2015-01-28||no|41|1.0.0-pre|40.0.2214.91|13098|
|[v0.20.8](https://github.com/electron/electron/releases/tag/v0.20.8)|2015-01-27||no|17|0.13.0-pre|39.0.2171.65|266|
|[v0.20.7](https://github.com/electron/electron/releases/tag/v0.20.7)|2015-01-20||no|17|0.13.0-pre|39.0.2171.65|1849|
|[v0.20.6](https://github.com/electron/electron/releases/tag/v0.20.6)|2015-01-19||no|17|0.13.0-pre|39.0.2171.65|1473|
|[v0.20.5](https://github.com/electron/electron/releases/tag/v0.20.5)|2015-01-08||no|17|0.13.0-pre|39.0.2171.65|4660|
|[v0.20.4](https://github.com/electron/electron/releases/tag/v0.20.4)|2015-01-06||no|17|0.13.0-pre|39.0.2171.65|1428|
|[v0.20.3](https://github.com/electron/electron/releases/tag/v0.20.3)|2014-12-29||no|17|0.13.0-pre|39.0.2171.65|2909|
|[v0.20.2](https://github.com/electron/electron/releases/tag/v0.20.2)|2014-12-22||no|17|0.13.0-pre|39.0.2171.65|2650|
|[v0.20.1](https://github.com/electron/electron/releases/tag/v0.20.1)|2014-12-18||no|17|0.13.0-pre|39.0.2171.65|1205|
|[v0.20.0](https://github.com/electron/electron/releases/tag/v0.20.0)|2014-12-13||no|17|0.13.0-pre|39.0.2171.65|1109|
|[v0.19.5](https://github.com/electron/electron/releases/tag/v0.19.5)|2014-11-28||no||||7237|
|[v0.19.4](https://github.com/electron/electron/releases/tag/v0.19.4)|2014-11-21||no||||7447|
|[v0.19.3](https://github.com/electron/electron/releases/tag/v0.19.3)|2014-11-20||no||||353|
|[v0.19.2](https://github.com/electron/electron/releases/tag/v0.19.2)|2014-11-15||no||||2865|
|[v0.19.1](https://github.com/electron/electron/releases/tag/v0.19.1)|2014-11-04||no||||4219|
|[v0.19.0](https://github.com/electron/electron/releases/tag/v0.19.0)|2014-10-30||no||||1055|
|[v0.18.2](https://github.com/electron/electron/releases/tag/v0.18.2)|2014-10-21||no||||4427|
|[v0.18.1](https://github.com/electron/electron/releases/tag/v0.18.1)|2014-10-17||no||||2272|
|[v0.18.0](https://github.com/electron/electron/releases/tag/v0.18.0)|2014-10-14||no||||2024|
|[v0.17.2](https://github.com/electron/electron/releases/tag/v0.17.2)|2014-10-06||no||||1723|
|[v0.17.1](https://github.com/electron/electron/releases/tag/v0.17.1)|2014-10-01||no||||4080|
|[v0.17.0](https://github.com/electron/electron/releases/tag/v0.17.0)|2014-10-01||no||||310|
|[v0.16.3](https://github.com/electron/electron/releases/tag/v0.16.3)|2014-09-20||no||||1141|
|[v0.16.2](https://github.com/electron/electron/releases/tag/v0.16.2)|2014-09-09||no||||5525|
|[v0.16.1](https://github.com/electron/electron/releases/tag/v0.16.1)|2014-09-08||no||||462|
|[v0.16.0](https://github.com/electron/electron/releases/tag/v0.16.0)|2014-09-06||no||||409|
|[v0.15.9](https://github.com/electron/electron/releases/tag/v0.15.9)|2014-08-20||no||||6557|
|[v0.15.8](https://github.com/electron/electron/releases/tag/v0.15.8)|2014-08-18||no||||8080|
|[v0.15.7](https://github.com/electron/electron/releases/tag/v0.15.7)|2014-08-15||no||||8989|
|[v0.15.6](https://github.com/electron/electron/releases/tag/v0.15.6)|2014-08-13||no||||10323|
|[v0.15.5](https://github.com/electron/electron/releases/tag/v0.15.5)|2014-08-11||no||||7995|
|[v0.15.4](https://github.com/electron/electron/releases/tag/v0.15.4)|2014-08-07||no||||8374|
|[v0.15.3](https://github.com/electron/electron/releases/tag/v0.15.3)|2014-08-06||no||||10116|
|[v0.15.2](https://github.com/electron/electron/releases/tag/v0.15.2)|2014-08-04||no||||7853|
|[v0.15.1](https://github.com/electron/electron/releases/tag/v0.15.1)|2014-07-31||no||||8292|
|[v0.15.0](https://github.com/electron/electron/releases/tag/v0.15.0)|2014-07-29||no||||8045|
|[v0.14.3](https://github.com/electron/electron/releases/tag/v0.14.3)|2014-07-27||no||||7559|
|[v0.14.2](https://github.com/electron/electron/releases/tag/v0.14.2)|2014-07-25||no||||7395|
|[v0.14.1](https://github.com/electron/electron/releases/tag/v0.14.1)|2014-07-24||no||||7365|
|[v0.14.0](https://github.com/electron/electron/releases/tag/v0.14.0)|2014-07-22||no||||7605|
|[v0.13.3](https://github.com/electron/electron/releases/tag/v0.13.3)|2014-06-25||no||||15391|
|[v0.13.2](https://github.com/electron/electron/releases/tag/v0.13.2)|2014-06-18||no||||4083|
|[v0.13.1](https://github.com/electron/electron/releases/tag/v0.13.1)|2014-06-14||no||||2026|
|[v0.13.0](https://github.com/electron/electron/releases/tag/v0.13.0)|2014-06-05||no||||3876|
|[v0.12.7](https://github.com/electron/electron/releases/tag/v0.12.7)|2014-05-27||no||||4764|
|[v0.12.6](https://github.com/electron/electron/releases/tag/v0.12.6)|2014-05-26||no||||379|
|[v0.12.5](https://github.com/electron/electron/releases/tag/v0.12.5)|2014-05-19||no||||21233|
|[v0.12.4](https://github.com/electron/electron/releases/tag/v0.12.4)|2014-05-12||no||||20135|
|[v0.12.3](https://github.com/electron/electron/releases/tag/v0.12.3)|2014-05-07||no||||6430|
|[v0.12.2](https://github.com/electron/electron/releases/tag/v0.12.2)|2014-05-05||no||||5069|
|[v0.12.1](https://github.com/electron/electron/releases/tag/v0.12.1)|2014-05-05||no||||277|
|[v0.12.0](https://github.com/electron/electron/releases/tag/v0.12.0)|2014-04-29||no||||254|
|[v0.11.10](https://github.com/electron/electron/releases/tag/v0.11.10)|2014-04-14||no||||172|
|[v0.11.9](https://github.com/electron/electron/releases/tag/v0.11.9)|2014-04-11||no||||161|
|[v0.11.8](https://github.com/electron/electron/releases/tag/v0.11.8)|2014-04-10||no||||165|
|[v0.11.7](https://github.com/electron/electron/releases/tag/v0.11.7)|2014-04-08||no||||162|
|[v0.11.6](https://github.com/electron/electron/releases/tag/v0.11.6)|2014-04-07||no||||163|
|[v0.11.5](https://github.com/electron/electron/releases/tag/v0.11.5)|2014-04-02||no||||171|
|[v0.11.4](https://github.com/electron/electron/releases/tag/v0.11.4)|2014-03-28||no||||249|
|[v0.11.3](https://github.com/electron/electron/releases/tag/v0.11.3)|2014-03-25||no||||237|
|[v0.11.2](https://github.com/electron/electron/releases/tag/v0.11.2)|2014-03-24||no||||242|
|[v0.11.1](https://github.com/electron/electron/releases/tag/v0.11.1)|2014-03-18||no||||241|
|[v0.11.0](https://github.com/electron/electron/releases/tag/v0.11.0)|2014-03-16||no||||236|
|[v0.10.7](https://github.com/electron/electron/releases/tag/v0.10.7)|2014-03-11||no||||277|
|[v0.10.6](https://github.com/electron/electron/releases/tag/v0.10.6)|2014-03-07||no||||241|
|[v0.10.5](https://github.com/electron/electron/releases/tag/v0.10.5)|2014-03-05||no||||233|
|[v0.10.4](https://github.com/electron/electron/releases/tag/v0.10.4)|2014-03-02||no||||230|
|[v0.10.3](https://github.com/electron/electron/releases/tag/v0.10.3)|2014-02-28||no||||312|
|[v0.10.2](https://github.com/electron/electron/releases/tag/v0.10.2)|2014-02-27||no||||274|
|[v0.10.1](https://github.com/electron/electron/releases/tag/v0.10.1)|2014-02-25||no||||260|
|[v0.10.0](https://github.com/electron/electron/releases/tag/v0.10.0)|2014-02-24||no||||86|
|[v0.9.3](https://github.com/electron/electron/releases/tag/v0.9.3)|2014-02-17||no||||3709|
|[v0.9.2](https://github.com/electron/electron/releases/tag/v0.9.2)|2014-02-12||no||||3722|
|[v0.9.1](https://github.com/electron/electron/releases/tag/v0.9.1)|2014-02-04||no||||3707|
|[v0.9.0](https://github.com/electron/electron/releases/tag/v0.9.0)|2014-02-02||no||||3695|
|[v0.8.7](https://github.com/electron/electron/releases/tag/v0.8.7)|2014-01-27||no||||3727|
|[v0.8.6](https://github.com/electron/electron/releases/tag/v0.8.6)|2014-01-23||no||||3708|
|[v0.8.5](https://github.com/electron/electron/releases/tag/v0.8.5)|2014-01-14||no||||3740|
|[v0.8.4](https://github.com/electron/electron/releases/tag/v0.8.4)|2014-01-13||no||||3717|
|[v0.8.3](https://github.com/electron/electron/releases/tag/v0.8.3)|2014-01-08||no||||1859|
|[v0.8.2](https://github.com/electron/electron/releases/tag/v0.8.2)|2014-01-07||no||||3788|
|[v0.8.1](https://github.com/electron/electron/releases/tag/v0.8.1)|2013-12-29||no||||3729|
|[v0.8.0](https://github.com/electron/electron/releases/tag/v0.8.0)|2013-12-27||no||||1855|
|[v0.7.6](https://github.com/electron/electron/releases/tag/v0.7.6)|2013-12-09||no||||3802|
|[v0.7.5](https://github.com/electron/electron/releases/tag/v0.7.5)|2013-12-05||no||||3714|
|[v0.7.4](https://github.com/electron/electron/releases/tag/v0.7.4)|2013-12-04||no||||3790|
|[v0.7.3](https://github.com/electron/electron/releases/tag/v0.7.3)|2013-11-29||no||||3803|
|[v0.7.2](https://github.com/electron/electron/releases/tag/v0.7.2)|2013-11-28||no||||3749|
|[v0.7.1](https://github.com/electron/electron/releases/tag/v0.7.1)|2013-11-28||no||||3737|
|[v0.7.0](https://github.com/electron/electron/releases/tag/v0.7.0)|2013-11-27||no||||3728|
|[v0.6.12](https://github.com/electron/electron/releases/tag/v0.6.12)|2013-11-22||no||||1862|
|[v0.6.11](https://github.com/electron/electron/releases/tag/v0.6.11)|2013-11-20||no||||1861|
|[v0.6.10](https://github.com/electron/electron/releases/tag/v0.6.10)|2013-11-11||no||||1872|
|[v0.6.9](https://github.com/electron/electron/releases/tag/v0.6.9)|2013-11-07||no||||1867|
|[v0.6.8](https://github.com/electron/electron/releases/tag/v0.6.8)|2013-11-05||no||||1848|
|[v0.6.7](https://github.com/electron/electron/releases/tag/v0.6.7)|2013-11-02||no||||1858|
|[v0.6.6](https://github.com/electron/electron/releases/tag/v0.6.6)|2013-10-28||no||||1870|
|[v0.6.5](https://github.com/electron/electron/releases/tag/v0.6.5)|2013-10-26||no||||1824|
|[v0.6.4](https://github.com/electron/electron/releases/tag/v0.6.4)|2013-10-22||no||||1858|
|[v0.6.3](https://github.com/electron/electron/releases/tag/v0.6.3)|2013-10-21||no||||908|
|[v0.6.2](https://github.com/electron/electron/releases/tag/v0.6.2)|2013-10-17||no||||910|
|[v0.6.1](https://github.com/electron/electron/releases/tag/v0.6.1)|2013-10-14||no||||912|
|[v0.6.0](https://github.com/electron/electron/releases/tag/v0.6.0)|2013-10-10||no||||913|
|[v0.5.4](https://github.com/electron/electron/releases/tag/v0.5.4)|2013-10-04||no||||938|
|[v0.5.3](https://github.com/electron/electron/releases/tag/v0.5.3)|2013-09-29||no||||898|
|[v0.5.2](https://github.com/electron/electron/releases/tag/v0.5.2)|2013-09-29||no||||900|
|[v0.5.1](https://github.com/electron/electron/releases/tag/v0.5.1)|2013-09-26||no||||919|
|[v0.5.0](https://github.com/electron/electron/releases/tag/v0.5.0)|2013-09-25||no||||907|
|[v0.4.9](https://github.com/electron/electron/releases/tag/v0.4.9)|2013-09-20||no||||898|
|[v0.4.8](https://github.com/electron/electron/releases/tag/v0.4.8)|2013-09-20||no||||897|
|[v0.4.7](https://github.com/electron/electron/releases/tag/v0.4.7)|2013-09-13||no||||898|
|[v0.4.6](https://github.com/electron/electron/releases/tag/v0.4.6)|2013-09-12||no||||901|
|[v0.4.5](https://github.com/electron/electron/releases/tag/v0.4.5)|2013-09-09||no||||902|
|[v0.4.4](https://github.com/electron/electron/releases/tag/v0.4.4)|2013-09-05||no||||901|
|[v0.4.3](https://github.com/electron/electron/releases/tag/v0.4.3)|2013-09-02||no||||901|
|[v0.4.2](https://github.com/electron/electron/releases/tag/v0.4.2)|2013-09-02||no||||1818|
|[v0.4.1](https://github.com/electron/electron/releases/tag/v0.4.1)|2013-08-27||no||||0|
|[v0.4.0](https://github.com/electron/electron/releases/tag/v0.4.0)|2013-08-19||no||||0|
|[v0.3.5](https://github.com/electron/electron/releases/tag/v0.3.5)|2013-08-16||no||||0|
|[v0.3.4](https://github.com/electron/electron/releases/tag/v0.3.4)|2013-08-15||no||||0|
|[v0.3.3](https://github.com/electron/electron/releases/tag/v0.3.3)|2013-08-15||no||||0|
|[v0.3.2](https://github.com/electron/electron/releases/tag/v0.3.2)|2013-08-13||no||||0|
|[v0.3.1](https://github.com/electron/electron/releases/tag/v0.3.1)|2013-08-12||no||||0|

<!-- END RELEASES TABLE -->
