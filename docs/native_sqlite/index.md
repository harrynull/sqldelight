# Getting started on Kotlin Native with SQLDelight

{% include 'common/index_gradle_database.md' %}

{% include 'common/index_schema.md' %}

=== "Kotlin"
    ```kotlin
    kotlin {
      // or sourceSets.iosMain, sourceSets.windowsMain, etc.
      sourceSets.nativeMain.dependencies {
        implementation("app.cash.sqldelight:native-driver:{{ versions.sqldelight }}")
      }
    }
    ```
=== "Groovy"
    ```groovy
    kotlin {
      // or sourceSets.iosMain, sourceSets.windowsMain, etc.
      sourceSets.nativeMain.dependencies {
        implementation "app.cash.sqldelight:native-driver:{{ versions.sqldelight }}"
      }
    }
    ```

```kotlin
val driver: SqlDriver = NativeSqliteDriver(Database.Schema, "test.db")
```

### Kotlin/Native Memory Models

The SQLDelight native driver is compatible with both the original strict memory model and the updated
memory model. However, it is optimized for the new memory model, and as most of the official Jetbrains
libraries will be gradually dropping support for the strict memory model, support for the strict
memory model may be deprecated or removed in future releases.

{% include 'common/index_queries.md' %}

## Reader Connection Pools

Disk databases can (optionally) have multiple reader connections. To configure the reader pool, pass the `maxReaderConnections` parameter to the various constructors of `NativeSqliteDriver`:

```kotlin
val driver: SqlDriver = NativeSqliteDriver(
    Database.Schema, 
    "test.db", 
    maxReaderConnections = 4
)
```

Reader connections are only used to run queries outside of a transaction. Any write calls, and anything in a transaction, 
uses a single connection dedicated to transactions.
