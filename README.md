# @gedeagas/react-native-bundle-visualizer

See what's inside of your react-native bundle 📦

![bundle-visualizer-animation](./react-native-bundle-visualizer2.gif)

Uses the awesome [source-map-explorer](https://github.com/danvk/source-map-explorer) to visualize the output of the [Metro bundler](https://github.com/facebook/metro).

## Purpose

Sometimes, importing a single javascript library can drastically increase your bundle size. This package helps you to identify such a library, so you can keep the bundle size low and loading times fast.

## Usage

Make sure [npx](https://github.com/npm/npx) is installed and run the following command in your project root

`npx @gedeagas/react-native-bundle-visualizer`

### Or install as a dev-dependency

```sh
yarn add --dev @gedeagas/react-native-bundle-visualizer
```

And run it:

```
yarn run react-native-bundle-visualizer
```

_or when using npm:_

```
npm install --save-dev @gedeagas/react-native-bundle-visualizer
npx react-native-bundle-visualizer
```

## CI/CD Usage

For CI/CD environments where you need to capture just the bundle size for comparison or monitoring, use the `--bundle-size-only` option (outputs 3 decimal places):

```bash
# Output only the bundle size in megabytes (e.g., "2.450MB")
npx @gedeagas/react-native-bundle-visualizer --bundle-size-only

# Example: Capture bundle size in a variable
BUNDLE_SIZE=$(npx @gedeagas/react-native-bundle-visualizer --bundle-size-only)
echo "Bundle size: $BUNDLE_SIZE"

# Example: Compare bundle sizes in CI
PREV_SIZE=$(cat .bundle-size-cache 2>/dev/null || echo "0MB")
CURRENT_SIZE=$(npx @gedeagas/react-native-bundle-visualizer --bundle-size-only)
echo $CURRENT_SIZE > .bundle-size-cache

# Extract numeric values for comparison (removes "MB" suffix)
PREV_NUM=$(echo $PREV_SIZE | sed 's/MB//')
CURRENT_NUM=$(echo $CURRENT_SIZE | sed 's/MB//')

if (( $(echo "$CURRENT_NUM > $PREV_NUM" | bc -l) )); then
  echo "⚠️  Bundle size increased from $PREV_SIZE to $CURRENT_SIZE"
  exit 1
else
  echo "✅ Bundle size is $CURRENT_SIZE (was $PREV_SIZE)"
fi
```

## Command line arguments

All command-line arguments are optional. By default a production build will be created for the `ios` platform.

| Option               | Description                                                                                                                                                                   | Example                          |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| `platform`           | Platform to build (default is **ios**)                                                                                                                                        | `--platform ios`                 |
| `dev`                | Dev or production build (default is **false**)                                                                                                                                | `--dev false`                    |
| `entry-file`         | Entry-file (when omitted tries to auto-resolve it)                                                                                                                            | `--entry-file ./index.ios.js`    |
| `bundle-output`      | Output bundle-file (default is **tmp**)                                                                                                                                       | `--bundle-output ./myapp.bundle` |
| `format`             | Output format **html**, **json** or **tsv** (default is **html**) (see [source-map-explorer options][smeo])                                                                   | `--format json`                  |
| `only-mapped`        | Exclude "unmapped" bytes from the output (default is **false**). This will result in total counts less than the file size.                                                    | `--only-mapped`                  |
| `verbose`            | Dumps additional output to the console (default is **false**)                                                                                                                 | `--verbose`                      |
| `reset-cache`        | Removes cached react-native files (default is **false**)                                                                                                                      | `--reset-cache`                  |
| `--expo`             | Set this to true/ false based on whether using expo or not. For eg, set `--expo true` when using expo. Not required to pass this for react-native cli. (default is **false**) | `--expo false`                   |
| `--no-border-checks` | Pass the same flag to the underlying `source-map-explorer` to disable invalid mapping column/line checks.                                                                     | `--no-border-checks`             |
| `--bundle-size-only` | Output only the bundle size in plain megabytes with "MB" suffix (3 decimal places, e.g., "2.450MB") (useful for CI/CD environments). This will suppress all other output and return just the formatted size.                      | `--bundle-size-only`             |

[smeo]: https://github.com/danvk/source-map-explorer#options

> Use [react-native-bundle-visualizer@2](https://github.com/IjzerenHein/react-native-bundle-visualizer/tree/v2) when targetting Expo SDK 40 or lower.

## Version compatibility

| Version                                                                      | Comments                                                                                        |
| ---------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| 3.x                                                                          | Compatible with React-Native CLI bootstrapped projects and Expo SDK 41 or higher.               |
| [2.x](https://github.com/IjzerenHein/react-native-bundle-visualizer/tree/v2) | Compatible with React-Native CLI bootstrapped projects and Expo SDK 40 or earlier.              |
| [1.x](https://github.com/IjzerenHein/react-native-bundle-visualizer/tree/v1) | Uses the [Haul bundler](https://github.com/callstack/haul) instead instead of the Metro output. |

## License

[MIT](./LICENSE.txt)
