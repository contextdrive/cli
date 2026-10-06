# ctx

Command-line client for ContextDrive ([contextdrive.io](https://contextdrive.io)).

`ctx` is a thin client. `ctx login` stores a credential. `ctx ingest` walks a local tree, hashes it, asks `server` for a file row and an upload grant, writes the bytes to R2, and commits. `ctx status` reads the job. Parsing, embeddings, SQL, and policy stay in `contextdrive/server`. The client does not choose a bucket. `ctx` is Java, a native image, and it does not link DuckDB or Tika.

Product vision: private repo `contextdrive/vision` on `main`. How to read it, and the command boundaries, are in [AGENTS.md](AGENTS.md).

What `main` runs today is the Quarkus Picocli sample (`GreetingCommand`). The three commands above are the open issues.

This project uses Quarkus. See <https://quarkus.io/>.

## Running the application in dev mode

You can run your application in dev mode that enables live coding using:

```shell script
./mvnw quarkus:dev
```

> **_NOTE:_**  Quarkus now ships with a Dev UI, which is available in dev mode only at <http://localhost:8080/q/dev/>.

## Packaging and running the application

The application can be packaged using:

```shell script
./mvnw package
```

It produces the `quarkus-run.jar` file in the `target/quarkus-app/` directory.
Be aware that it’s not an _über-jar_ as the dependencies are copied into the `target/quarkus-app/lib/` directory.

The application is now runnable using `java -jar target/quarkus-app/quarkus-run.jar`.

If you want to build an _über-jar_, execute the following command:

```shell script
./mvnw package -Dquarkus.package.jar.type=uber-jar
```

The application, packaged as an _über-jar_, is now runnable using `java -jar target/*-runner.jar`.

## Creating a native executable

You can create a native executable using:

```shell script
./mvnw package -Dnative
```

Or, if you don't have GraalVM installed, you can run the native executable build in a container using:

```shell script
./mvnw package -Dnative -Dquarkus.native.container-build=true
```

You can then execute your native executable with: `./target/cli-1.0.0-SNAPSHOT-runner`

If you want to learn more about building native executables, please consult <https://quarkus.io/guides/maven-tooling>.

## Related Guides

- Picocli ([guide](https://quarkus.io/guides/picocli)): Develop command line applications with Picocli

## Sample on main

`GreetingCommand` is the Quarkus Picocli sample. The issues in this repo replace it with `ctx login`, `ctx ingest`, and `ctx status`.

Dev mode restarts the command when you press Enter. Pass arguments with:

```shell
./mvnw quarkus:dev -Dquarkus.args='Quarky'
```
