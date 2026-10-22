# Maven Fundamentals for DevOps

## 1. What Is Maven?

Apache Maven is a **build automation and dependency management tool** primarily used for Java projects.

Maven helps automate tasks such as:

- Compiling source code
- Running tests
- Packaging applications into JAR or WAR files
- Managing project dependencies
- Installing build artifacts into a local repository

### Important Terms

| Term | Meaning |
|---|---|
| Build | The process of transforming source code into usable output |
| Dependency | An external library required by a project |
| Artifact | A file produced or managed by a build, such as a JAR |
| JAR | Java Archive containing compiled classes and related resources |
| POM | Project Object Model; Maven's project configuration file |
| Lifecycle | An ordered sequence of build phases |
| Repository | A location where Maven stores or retrieves artifacts |

## 2. Maven Installation and Verification

### Environment Used

- Operating system: Ubuntu on WSL2
- Java version: OpenJDK 21
- Maven version: 3.9.12

### Verify Java

```bash
java --version
javac --version
```

- `java --version` displays the Java runtime version.
- `javac --version` displays the Java compiler version.

### Install Maven on Ubuntu

```bash
sudo apt update
sudo apt install maven -y
```

### Verify Maven

```bash
mvn --version
```

This displays the Maven version, Maven installation directory, Java version, Java runtime path, and operating-system details.

**DevOps takeaway:** Verify Java and Maven versions before troubleshooting a build environment.

## 3. Standard Maven Project Structure

Our practice project used this structure:

```text
maven-devops-demo/
├── pom.xml
├── src/
│   └── main/
│       └── java/
│           └── App.java
└── target/
    ├── classes/
    │   └── App.class
    └── maven-devops-demo-1.0-SNAPSHOT.jar
```

The `target/` directory is generated during the build and can be recreated.

| File or Directory | Purpose |
|---|---|
| `pom.xml` | Maven project configuration |
| `src/main/java/` | Application Java source code |
| `src/main/resources/` | Application resources, if present |
| `src/test/java/` | Test source code, if present |
| `src/test/resources/` | Test resources, if present |
| `target/` | Generated build output |
| `target/classes/` | Compiled application classes |
| `.jar` | Packaged Java archive |

Not every directory must exist in every project. Maven can report that a source or resource directory is missing when that directory has not been created; this is not automatically a build failure.

## 4. Understanding `pom.xml`

The POM is the main Maven project configuration file.

Our project used this configuration:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>maven-devops-demo</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.release>21</maven.compiler.release>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

</project>
```

### Important POM Fields

| Field | Purpose |
|---|---|
| `modelVersion` | Identifies the POM model version |
| `groupId` | Identifies the project's group or organization |
| `artifactId` | Identifies the project or artifact |
| `version` | Specifies the project version |
| `maven.compiler.release` | Configures the Java release targeted by the compiler |
| `project.build.sourceEncoding` | Sets the source-file encoding |

Our project coordinates were:

- Group ID: `com.example`
- Artifact ID: `maven-devops-demo`
- Version: `1.0-SNAPSHOT`

`SNAPSHOT` commonly identifies a development version that may change during development.

**DevOps takeaway:** A DevOps engineer should understand the basic POM fields and be able to inspect configuration when troubleshooting a build.

## 5. Maven Build Lifecycle

Maven uses predefined build lifecycles. The default lifecycle includes the following important phases:

| Phase | Purpose |
|---|---|
| `validate` | Checks that the project is correctly configured for the build |
| `compile` | Compiles application source code |
| `test` | Runs available tests |
| `package` | Packages compiled code into a JAR or WAR |
| `verify` | Performs additional verification checks configured for the build |
| `install` | Installs the artifact into the local Maven repository |
| `deploy` | Publishes the artifact to a remote repository |

The simplified sequence is:

```text
validate
   ↓
compile
   ↓
test
   ↓
package
   ↓
verify
   ↓
install
   ↓
deploy
```

This is a simplified view of the default lifecycle; Maven has additional phases.

### Important Rule

When you request a later phase, Maven executes the required earlier phases in that lifecycle.

For example:

```bash
mvn install
```

runs the required phases leading up to `install`. You do not need to run `compile`, `test`, and `package` separately beforehand.

Maven may reuse up-to-date outputs in some situations, but that does not mean the lifecycle phases are omitted.

## 6. Essential Maven Commands

### Check Maven Installation

```bash
mvn --version
```

Displays Maven and Java version information.

### Compile the Application

```bash
mvn compile
```

Compiles application source code.

Our compiled class appeared at:

```text
target/classes/App.class
```

### Run Available Tests

```bash
mvn test
```

Runs the required earlier build phases and then executes available tests.

Our project reported:

```text
[INFO] No tests to run.
[INFO] BUILD SUCCESS
```

This happened because we had not created test classes.

**Important:** `BUILD SUCCESS` with no tests does not prove that the application's behaviour has been tested.

### Package the Application

```bash
mvn package
```

Runs the required earlier phases and creates the package.

Our generated artifact was:

```text
target/maven-devops-demo-1.0-SNAPSHOT.jar
```

### Install the Artifact Locally

```bash
mvn install
```

Runs the required earlier phases and installs the project's artifact and POM into the local Maven repository.

### Clean the Build Output

```bash
mvn clean
```

Removes generated build output, normally the `target/` directory.

It does not normally delete:

- `src/`
- `pom.xml`
- The local Maven repository at `~/.m2/repository/`

### Clean, Rebuild, and Install

```bash
mvn clean install
```

This command:

1. Cleans the previous build output.
2. Compiles the source code.
3. Runs available tests.
4. Packages the application.
5. Runs the remaining required phases through installation.
6. Installs the artifact and POM in the local Maven repository.

This is useful when you want a fresh build followed by local installation.

## 7. Difference Between `package` and `install`

| Command | Main result |
|---|---|
| `mvn package` | Creates the JAR or WAR in `target/` |
| `mvn install` | Performs the required earlier phases and also installs the artifact in the local Maven repository |

The local repository is useful when another local Maven project needs an artifact that has been installed on the same machine.

**Important:** `mvn install` does not automatically publish the artifact to a remote repository or deploy the application.

## 8. Maven Dependencies

A dependency is an external library that an application requires.

Examples include libraries for database access, HTTP communication, JSON processing, and testing.

Dependencies are normally declared in `pom.xml` inside a `<dependencies>` section.

A dependency declaration identifies a library using:

- `groupId`
- `artifactId`
- `version`

### Direct Dependency

A library explicitly declared by the project in its POM.

### Transitive Dependency

A library required by another dependency and brought in through dependency resolution.

For example:

```text
Application
    ↓
Library A (direct dependency)
    ↓
Library B (transitive dependency)
```

If Library A requires Library B, Maven can resolve Library B automatically, provided its dependency information and required artifacts are available.

**DevOps takeaway:** Dependency errors may involve direct dependencies or their transitive dependencies.

## 9. Maven Repositories

Maven uses repositories to store and retrieve artifacts.

### Local Repository

The default local repository is generally:

```text
~/.m2/repository/
```

It stores downloaded dependencies and artifacts installed by local builds.

Our project was installed under:

```text
~/.m2/repository/com/example/maven-devops-demo/1.0-SNAPSHOT/
```

The directory contained:

```text
maven-devops-demo-1.0-SNAPSHOT.jar
maven-devops-demo-1.0-SNAPSHOT.pom
maven-metadata-local.xml
```

### Remote Repository

A remote repository provides artifacts that Maven can retrieve when needed. Maven Central is a widely used public repository.

Maven checks its local repository and can retrieve missing artifacts from configured remote repositories when they are available and accessible.

### `target/` vs. `~/.m2/repository/`

| Location | Purpose |
|---|---|
| `target/` | Generated output for the current project build |
| `~/.m2/repository/` | Locally cached dependencies and installed artifacts |

Running `mvn clean` removes generated build output, not the local Maven repository.

## 10. Inspecting and Running Build Output

### List Generated Artifacts

```bash
ls -lh target/*.jar
```

Lists matching JAR files and displays their sizes.

### Inspect a JAR

```bash
jar tf target/maven-devops-demo-1.0-SNAPSHOT.jar
```

Lists the contents of the JAR without extracting it.

Our JAR contained:

```text
META-INF/MANIFEST.MF
App.class
META-INF/maven/com.example/maven-devops-demo/pom.xml
META-INF/maven/com.example/maven-devops-demo/pom.properties
```

- `App.class` is the compiled application class.
- `META-INF/MANIFEST.MF` contains JAR manifest metadata.
- The embedded POM and properties provide Maven project metadata.

A JAR containing a manifest is not necessarily executable with `java -jar`; an appropriate `Main-Class` entry is needed for that launch method.

### Run the Compiled Class

```bash
java -cp target/classes App
```

Our application printed:

```text
Hello from Maven!
```

Command breakdown:

- `java` starts the Java runtime.
- `-cp` specifies the classpath.
- `target/classes` contains compiled classes.
- `App` is the class to execute.

This command runs the compiled class directly; it does not execute the JAR.

## 11. Maven Build Troubleshooting

### A. Dependency-Resolution Failure

Possible messages:

```text
Could not resolve dependencies
Could not transfer artifact
Connection timed out
```

Investigate:

1. Dependency coordinates and version.
2. Repository availability.
3. Network connectivity and DNS.
4. Proxy and firewall configuration.
5. Repository credentials, if required.

Do not immediately delete the local Maven repository. First identify the underlying cause.

### B. Compilation Failure

Possible messages:

```text
COMPILATION ERROR
cannot find symbol
```

Investigate:

1. The first relevant compiler error.
2. The affected source file and line number.
3. Incorrect names, missing imports, or unavailable classes.
4. Missing dependencies or incompatible configuration, where applicable.

### C. `No tests to run`

This can occur when no test classes are present.

It is not necessarily a build failure, but it means the project has not demonstrated successful execution of application tests.

### D. `BUILD SUCCESS` and `BUILD FAILURE`

- `BUILD SUCCESS` means Maven completed the requested build without a build failure.
- `BUILD FAILURE` means the requested build failed.

Always inspect the surrounding log messages to determine what happened. A successful build does not automatically prove that the application is functionally correct.

## 12. Practical Workflow

For a project where you need to clean, build, test, package, and install the artifact locally:

```bash
mvn clean install
```

Then inspect the generated JAR:

```bash
ls -lh target/*.jar
jar tf target/maven-devops-demo-1.0-SNAPSHOT.jar
```

For the practice project, the compiled class can be run with:

```bash
java -cp target/classes App
```

Expected output:

```text
Hello from Maven!
```

## 13. Interview Questions and Answers

### What is Maven?

Maven is a build automation and dependency management tool primarily used for Java projects. It uses a POM file to manage project configuration, dependencies, and build behaviour.

### What is `pom.xml`?

`pom.xml` is the Project Object Model file. It defines project coordinates, dependencies, properties, and other build configuration.

### What is the difference between `mvn package` and `mvn install`?

`mvn package` creates the packaged artifact in the project's build output directory. `mvn install` also installs the artifact and POM into the local Maven repository.

### What does `mvn clean` do?

It removes generated build output, normally the `target/` directory, so a subsequent build can regenerate those files.

### What is a Maven dependency?

A dependency is an external library required by a project. Maven resolves and manages dependencies declared in the POM.

### What is the local Maven repository?

It is the local storage location, normally `~/.m2/repository/`, where Maven keeps downloaded dependencies and locally installed artifacts.

### What is a JAR?

A JAR is a Java Archive that packages compiled classes and potentially other resources and metadata.

### What would you investigate if a Maven build failed while downloading a dependency?

I would inspect the error message, verify dependency coordinates and repository configuration, and check network connectivity, DNS, proxy/firewall rules, and credentials where relevant.

### Does `BUILD SUCCESS` mean all tests passed?

Not necessarily. The build might have no tests to execute. I would inspect the test results and confirm that tests actually ran.

## 14. Final Summary

The essential Maven concepts for this learning stage are:

- Understand `pom.xml` and standard project structure.
- Know the build lifecycle and the purpose of each essential command.
- Understand direct and transitive dependencies.
- Distinguish build output from the local Maven repository.
- Identify the generated JAR and inspect its contents.
- Read Maven error messages and troubleshoot based on evidence.
- Verify build success without assuming that a successful build proves functional correctness.

**Practical environment:** Ubuntu on WSL2, OpenJDK 21, Apache Maven 3.9.12.

**Practice project:** `maven-devops-demo`

**Status:** Maven fundamentals for DevOps — completed.
