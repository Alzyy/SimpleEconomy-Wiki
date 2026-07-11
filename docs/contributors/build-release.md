# Build and Release

SimpleEconomy is split into two Gradle subprojects:

- `plugin` builds the shaded Spigot/Paper plugin JAR.
- `api` builds and publishes the standalone API artifact.

## Shared Build Settings

- All subprojects compile against Java 21.
- Resources expand `${version}` placeholders into plugin metadata files.

## Plugin Build

The plugin module uses the Shadow plugin to produce the runtime JAR.

- Output name: `SimpleEconomy.jar`
- Includes the ACF command framework, bStats, HikariCP, and other shaded runtime dependencies
- Tests run before the shaded archive is created

Suggested command:

```bash
./gradlew :plugin:shadowJar
```

## API Build

The API module publishes `io.github.alzyy:simpleeconomy-api:1.0.2` with sources and Javadoc JARs.

The API build defines these release tasks:

- `generateChecksums`
- `buildBundle`

### `generateChecksums`

- Depends on `build` and `signMavenJavaPublication`
- Generates MD5 and SHA1 checksum files for the built JARs and POM
- Covers the runtime JAR, sources JAR, Javadoc JAR, and published POM

### `buildBundle`

- Depends on `build`, `signMavenJavaPublication`, and `generateChecksums`
- Produces a ZIP bundle under `api/build/bundle/`
- Stages the signed Maven artifacts in the layout required for release upload

Suggested command:

```bash
./gradlew :api:buildBundle
```

## Release Notes

- The root project version is `3.6`.
- The API module version is `1.0.2`.
- The plugin metadata files are expanded from Gradle properties during resource processing.