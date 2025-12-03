# Gradle Capability Conflict with lz4-java Relocation

Minimal reproduction for [yawkat/lz4-java#11](https://github.com/yawkat/lz4-java/issues/11).

When depending on `org.lz4:lz4-java:1.8.1`, Gradle fails with a capability conflict.

## Reproduction

```bash
./gradlew dependencies --configuration compileClasspath
```

```
compileClasspath - Compile classpath for source set 'main'.
\--- org.lz4:lz4-java:1.8.1 FAILED

> Could not resolve org.lz4:lz4-java:1.8.1.
  > Module 'org.lz4:lz4-java' has been rejected:
       Cannot select module with conflict on capability 'org.lz4:lz4-java:1.8.1'
       also provided by [at.yawk.lz4:lz4-java:1.8.1(apiElements)]
```

## Cause

Gradle treats the relocation target as a dependency ([gradle/gradle#1256](https://github.com/gradle/gradle/issues/1256)). Both `org.lz4:lz4-java` and `at.yawk.lz4:lz4-java` then claim the `org.lz4:lz4-java` capability, causing a conflict.

## Workarounds

See commented-out code in `build.gradle`.
