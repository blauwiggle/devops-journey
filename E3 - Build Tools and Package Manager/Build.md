# Build Tools and Package Manager

## EXERCISE 0: Clone project and create own Git repository

```bash
git clone https://gitlab.com/devops-bootcamp3/java-gradle-app.git
cd java-gradle-app
git remote add origin https://github.com/blauwiggle/devops-journey-e3.git
git branch -M main
git push -u origin main
```

## EXERCISE 1: Build jar artifact

```bash
./gradlew build
```

## EXERCISE 2: Run tests

Fix the test by changing `"true"` to `true` in the relevant test file.

```bash
./gradlew test
```

## EXERCISE 3: Clean and build App

```bash
./gradlew clean
./gradlew build
```

## EXERCISE 4: Start application

```bash
java -jar build/libs/app-1.0.jar
```

## EXERCISE 5: Start App with 2 Parameters

Add the following code snippet to `Application.java on line 16:

```java
Logger log = LoggerFactory.getLogger(Application.class);
try {
    String one = args[0];
    String two = args[1];
    log.info("Application will start with the parameters {} and {}", one, two);
} catch (Exception e) {
    log.info("No parameters provided");
}
```
Rebuild the jar file:

```bash
./gradlew build
```

Execute the jar file again with 2 params:

```bash
java -jar build/libs/app-1.0.jar param1 param2
```
