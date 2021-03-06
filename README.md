# badge-matrix

More advanced badges for your projects using Travis or Sauce Labs.

## Badges

#### File size badges for any file on GitHub or npm

[![Builder package.json size](https://badges.herokuapp.com/size/github/FormidableLabs/builder/master/package.json)](https://github.com/FormidableLabs/builder)

[![Victory size](https://badges.herokuapp.com/size/npm/victory/dist/victory.min.js?gzip=true)](https://www.npmjs.com/package/victory)


#### Status badges for slices of your Travis build matrix
[![NPM_3=true Status](https://badges.herokuapp.com/travis/FormidableLabs/builder?env=NPM_3=true&label=NPM_3=true)](https://travis-ci.org/FormidableLabs/builder)
[![NPM_3=false Status](https://badges.herokuapp.com/travis/FormidableLabs/builder?env=NPM_3=false&label=NPM_3=false)](https://travis-ci.org/FormidableLabs/builder)

#### Browser matrix badges for Sauce Labs
[![Browser Status](https://badges.herokuapp.com/sauce/wml-little-loader)](https://saucelabs.com/u/wml-little-loader)

Beautiful *and* customizable!

* `labels=none`

  [![Browser Status](https://badges.herokuapp.com/sauce/wml-little-loader?labels=none)](https://saucelabs.com/u/wml-little-loader)
* `logos=none`

  [![Browser Status](https://badges.herokuapp.com/sauce/wml-little-loader?logos=none)](https://saucelabs.com/u/wml-little-loader)
* `logos=none&labels=longName`

  [![Browser Status](https://badges.herokuapp.com/sauce/wml-little-loader?logos=none&labels=longName)](https://saucelabs.com/u/wml-little-loader)

## Endpoints

Deployed at `https://badges.herokuapp.com/`

* `/size/:source/:path`

  Render a file size badge for any file on GitHub or npm.

  * `:source` can be `github` or `npm`.
  * `:path` can be any valid `raw.githubusercontent.com` or `npmcdn.com` path
    (when `:source` is `github` or `npm`, respectively).

  **Query parameters**

  * `gzip`

    Whether to show the gzip-compressed size, defaults to **false**.

    * **true**: Show compressed size.
    * **false**: Show uncompressed size.

  * `label`

    Custom badge label, by default it will be "size" or "size (gzip)".
    
  * `color`
  
    Color name or value to pass along to [shields.io](http://shields.io/),
    defaults to **brightgreen**. Note that the default may change to **blue** in
    the future, as is somewhat conventional for purely informational,
    non-qualitative badges like this one.

* `/sauce/:user`

  Render browser support matrix badge for the Sauce Labs account at `:user`.

  **Query parameters**

  * `build`

    Build number, it should match the `build` string of one or more jobs. By
    default, try to find the most recent build. The build can be from any CI
    service, not just Travis.

    Sauce Labs’ API doesn’t allow filtering by build, so finding the jobs for a
    build can be a bit of a hassle:

    * If the requested build is not in the first 200 results returned by the
      API, then you should specify `from` and `to` to limit the query window
      to the time span of the build.
    * If no `from` is given, then stop fetching more jobs from the API when a
      different build number is encountered.

    Jobs with a `null` value for `build` are never included.
  * `name`

    Name filter, it should match a whitespace separated substring in the `name`
    of one or more jobs. Only jobs matching the filter will be included in the
    result.
  * `tag`

    Tag filter, it should match a string in the `tags` array of one or more
    jobs. Only jobs matching the filter will be included in the result.
  * `from`

    Start time (Unix epoch) of the window in which to find jobs. Passed along
    to the Sauce Labs API.
  * `to`

    End time (Unix epoch) of the window in which to find jobs. Passed along to
    the Sauce Labs API.
  * `skip`

    Number of initial jobs to skip. Passed along to the Sauce Labs API.
  * `logos`

    How to render browser logos, defaults to **inside**.

    * **inside** or **true**: Show logos in the label part of the badge.
    * **none** or **false**: Don’t show logos.
  * `labels`

    How to render browser labels, defaults to **shortName**.

    * **shortName** or **true**: Short names, e.g. "Chrome", "FF", "IE".
    * **name**: Medium names, e.g. "Chrome", "Firefox", "Internet Explorer".
    * **longName**: Long names, e.g. "Google Chrome", "Mozilla Firefox",
      "Microsoft Internet Explorer".
    * **sauceName**: Browser identifiers used by Sauce Labs, e.g.
      "googlechrome", "firefox", "iexplore".
    * **none** or **false**: Don’t show labels.
  * `versionDivider`

    How to render the divider between browser version numbers, defaults to
    **none**.

    * **none** or **false**: Don’t show a divider.
    * **line** or **true**: Show a subtle beveled line between version numbers.
* `/travis/:user/:repo`

  Render build status badge for the Travis project at `:user/:repo`, counting
  only build jobs that match the given `env` filter.

  **Query parameters**

  * `branch`

    Git branch, defaults to **master**.
  * `env`

    Environment filter, it should match a `VAR=value` line in the `env`
    section of your build matrix. All jobs in the build matching the filter
    will be aggregated into one final status, similar to how Travis determines
    an overall build status. If no filter is given, all jobs in the build are
    included (even if they are Allowed Failures).
  * `label`

    Text label to render on the left side of the badge, defaults to the repo
    name.
* `/travis/:user/:repo/sauce/:sauceUser`

  Render browser support matrix badge for the Travis project at `:user/:repo`,
  getting Sauce Labs results from `:sauceUser` (defaults to `:user`).

  You can also use the `/sauce/:user` endpoint, but this way ensures that we
  only consider Sauce Labs jobs that match up with the latest Travis build
  number for the given `branch`, and also makes the correct jobs easier to find
  since Travis provides the time span of the build.

  **Query parameters**

  * `branch`

    Git branch of the Travis build, defaults to **master**.
  * `name`

    `tag`

    `logos`

    `labels`

    `versionDivider`

    Same as the `/sauce/:user` endpoint above.
