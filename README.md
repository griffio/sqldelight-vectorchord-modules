# SqlDelight 2.2.x Postgresql VectorChord module support prototype 

https://github.com/cashapp/sqldelight

**Experimental**

Use with SqlDelight `2.2.1`

---

SqlDelight VectorChord Modules

https://github.com/tensorchord/VectorChord

https://github.com/tensorchord/VectorChord-bm25

https://docs.vectorchord.ai/

## Usage

Instead of a new dialect or adding PostgreSql extensions into the core PostgreSql grammar e.g. https://postgis.net/ and https://github.com/pgvector/pgvector

Use custom SqlDelight modules to implement grammar and type resolvers for VectorChord operations

```kotlin
sqldelight {
    databases {
        create("Sample") {
            deriveSchemaFromMigrations.set(true)
            migrationOutputDirectory = file("$buildDir/generated/migrations")
            migrationOutputFileFormat = ".sql"
            packageName.set("griffio.queries")
            dialect(libs.sqldelight.postgresql.dialect)
            module("io.github.griffio:sqldelight-bm25:0.0.2") // Parser rules are chained to allow ...
            module("io.github.griffio:sqldelight-vectorchord:0.0.2") // ... more than one module
        }
    }
}
```
---

Run the official VectorChord Postgresql Docker image for easier setup

```shell
docker run \
  --name vectorchord-demo \
  -e POSTGRES_PASSWORD=postgres \
  -p 5432:5432 \
  -d tensorchord/vchord-suite:pg18-latest
```

```shell
./gradlew build &&
./gradlew flywayMigrate
```
