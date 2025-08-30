## Reflection Access to JDK Internal Classes in JDK 9+

Since **JDK 9**, Java introduced the **module system (JPMS)**, which by default **encapsulates internal JDK packages**.  
This means that if your code tries to access certain internal JDK classes via reflection (e.g., private classes in the `java.util` package), you may encounter:

```java
java.lang.reflect.InaccessibleObjectException: Unable to make ... accessible
```

## Solution

During **run/debug**, you need to add a **JVM parameter** `--add-opens` to allow access to the specific module and package. For example:
```bash
--add-opens java.base/java.util=ALL-UNNAMED
```

Explanation:
- `java.base/java.util`: Target module/package
- `ALL-UNNAMED`: Allows access from all unnamed modules (your application)

---

## Configuring VM Options in IDEA

> By default, the VM options field is not visible in IDEA's run configuration and needs to be manually enabled.

1. Open **Run** → **Edit Configurations**

2. Select your application configuration

3. Locate the “**VM options**” input field (if not visible, click “**Modify options** → **Add VM options**”)

4. Enter:
```bash
-add-opens java.base/java.util=ALL-UNNAMED
```

5. Save and run. Reflection calls to internal JDK classes should now work properly.