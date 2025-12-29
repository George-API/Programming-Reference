# Java for Data Integration & Enterprise Backend

**Focus**: Enterprise backend patterns, data integration, I/O, JSON, HTTP, SOAP, concurrency, validation, logging, database access.

## Table of Contents

- [Built-In Syntax](#built-in-syntax)
- [Variables & Basic Types](#variables--basic-types)
- [Operators](#operators)
- [Control Flow](#control-flow)
- [Functions & Methods](#functions--methods)
- [Classes & OOP](#classes--oop)
- [Collections](#collections)
- [Exception Handling](#exception-handling)
- [File I/O](#file-io)
- [JSON Payloads](#json-payloads)
- [HTTP Client](#http-client)
- [SOAP Client](#soap-client)
- [Database Access](#database-access)
- [Concurrency Basics](#concurrency-basics)
- [Logging](#logging)
- [Validation](#validation)
- [Integration Patterns](#integration-patterns)

---

## Built-In Syntax

### Access modifiers
- `public` - accessible from anywhere
- `private` - accessible only within class
- `protected` - accessible within package and subclasses
- `package-private` - accessible within package (default)

### Class/interface keywords
- `class` - define class
- `interface` - define interface
- `abstract` - abstract class/method
- `final` - cannot be extended/overridden
- `static` - belongs to class, not instance
- `extends` - class inheritance
- `implements` - interface implementation

### Method/field modifiers
- `void` - no return value
- `return` - return value from method
- `this` - reference to current object
- `super` - reference to parent class
- `static` - class-level member
- `final` - cannot be reassigned/overridden

### Control flow
- `if` - conditional branch
- `else` - default branch
- `switch` - multi-way branch
- `case` - switch case label
- `default` - switch default case
- `for` - iteration loop
- `while` - loop while condition true
- `do` - do-while loop
- `break` - exit loop/switch
- `continue` - skip to next iteration

### Exception handling
- `try` - start exception handling
- `catch` - catch exception
- `finally` - always execute cleanup
- `throw` - throw exception
- `throws` - declare thrown exceptions

### Other keywords
- `import` - import package/class
- `package` - declare package
- `new` - create object instance
- `instanceof` - type check
- `null` - null reference
- `var` - local variable type inference (Java 10+)
- `record` - immutable data class (Java 14+)

---

## Variables & Basic Types

- **Primitives**: `int` (32-bit), `long` (64-bit, suffix `L`), `double` (64-bit), `float` (32-bit, suffix `f`), `boolean`, `char` (16-bit), `byte` (8-bit), `short` (16-bit)
- **Objects**: `Integer`, `Long`, `Double`, `Float`, `Boolean`, `Character`, `Byte`, `Short`, `String` (immutable)
- **Autoboxing**: Automatic conversion between primitives and wrappers
- **Type conversion**: `(int) 3.14` (explicit cast), `double y = 5` (implicit widening), `String.valueOf(42)`, `Integer.parseInt("42")`

---

## Operators

- **Arithmetic**: `+ - * / % ++ --`
- **Comparison**: `== != < > <= >=`, `instanceof` (type check)
- **Logical**: `&& || !`
- **Bitwise**: `& | ^ ~ << >> >>>` (unsigned right shift)
- **Assignment**: `= += -= *= /= %=`
- **Ternary**: `condition ? valueIfTrue : valueIfFalse`

---

## Control Flow

- **If/Else**: `if (cond) { } else if (cond) { } else { }`
- **Ternary**: `int max = (a > b) ? a : b;`
- **Switch**: `switch (val) { case 1: break; default: break; }` (traditional), `String result = switch (val) { case 1 -> "one"; default -> "other"; };` (expression, Java 14+)
- **For**: `for (int i = 0; i < 10; i++) { }` (traditional), `for (String item : items) { }` (enhanced/for-each), `items.forEach(System.out::println);` (stream)
- **While**: `while (cond) { }`, `do { } while (cond);` (executes at least once)

---

## Functions & Methods

- **Definition**: `public static int add(int a, int b) { return a + b; }`
- **Overloading**: Same method name, different parameters
- **Varargs**: `public int sum(int... numbers) { }` (variable arguments)
- **Lambdas** (Java 8+): `Function<Integer, Integer> square = x -> x * x;`, `Predicate<Integer> isEven = x -> x % 2 == 0;`, `Consumer<String> printer = s -> System.out.println(s);`
- **Method references**: `items.forEach(System.out::println);` (instance), `items.sort(String::compareToIgnoreCase);` (instance), `Integer::parseInt` (static)

---

## Classes & OOP

- **Class**: `public class User { private String name; public User(String name) { this.name = name; } public String getName() { return name; } }`
- **Records** (Java 14+): `public record UserEvent(String userId, String action, Instant timestamp) { }` (immutable data class, compact constructor for validation)
- **Inheritance**: `public class Admin extends User { public Admin(...) { super(...); } @Override public void greet() { super.greet(); } }`
- **Interface**: `public interface Service { void execute(); default void log(String msg) { } static Service create() { } }` (default/static methods, Java 8+)
- **Abstract class**: `public abstract class BaseHandler { public abstract void handle(); protected void validate() { } }`

---

## Collections (`java.util.*`)

- **List** (ordered, duplicates): `List<String> list = new ArrayList<>();` (mutable), `List.of("a", "b")` (immutable, Java 9+); `add()`, `get(i)`, `size()`
- **Set** (unique): `Set<String> set = new HashSet<>();` (hash, O(1)), `new LinkedHashSet<>()` (insertion order), `new TreeSet<>()` (sorted); `add()`, `contains()`
- **Map** (key-value): `Map<String, String> map = new HashMap<>();` (hash), `new LinkedHashMap<>()` (insertion order), `new TreeMap<>()` (sorted); `put()`, `get()`, `containsKey()`, `getOrDefault()`
- **Optional**: `Optional.ofNullable(val)`, `opt.orElse("default")`, `opt.orElseGet(() -> compute())`, `opt.ifPresent(v -> ...)`
- **Operations**: `add()`, `remove()`, `contains()`, `size()`, `isEmpty()`, `clear()`, `forEach()`, `stream()` (Java 8+)

---

## Exception Handling

- **Try-catch-finally**: `try { } catch (SpecificException e) { } catch (Exception e) { } finally { }` (always executes)
- **Try-with-resources**: `try (FileReader fr = new FileReader("file.txt"); BufferedReader br = new BufferedReader(fr)) { }` (auto-close)
- **Throw**: `throw new IllegalArgumentException("message");`
- **Custom**: `class IntegrationException extends RuntimeException { public IntegrationException(String msg, Throwable cause) { super(msg, cause); } }`
- **Declare**: `public void method() throws IOException, SQLException { }`
- **Types**: **Checked** (must declare: `IOException`, `SQLException`), **Unchecked** (`RuntimeException` subclasses: `IllegalArgumentException`, `NullPointerException`)

---

## File I/O (`java.nio.file.*`, `java.io.*`)

- **Read** (small): `String content = Files.readString(Paths.get("file.txt"), StandardCharsets.UTF_8);`
- **Read** (large, line-by-line): `try (Stream<String> lines = Files.lines(path, StandardCharsets.UTF_8)) { lines.forEach(...); }`
- **Write**: `Files.writeString(path, data, StandardCharsets.UTF_8);`, `Files.write(path, lines, StandardCharsets.UTF_8);`
- **Traditional**: `try (FileReader fr = new FileReader("file.txt"); BufferedReader br = new BufferedReader(fr)) { while ((line = br.readLine()) != null) { } }`
- **Write** (traditional): `try (FileWriter fw = new FileWriter("out.txt"); BufferedWriter bw = new BufferedWriter(fw)) { bw.write("text"); bw.newLine(); }`

---

## JSON Payloads (Jackson)

- **Setup**: `ObjectMapper MAPPER = new ObjectMapper().registerModule(new JavaTimeModule());` (for `LocalDateTime`, `Instant`)
- **Parse**: `public static <T> T fromJson(String json, Class<T> cls) { return MAPPER.readValue(json, cls); }`
- **Serialize**: `public static String toJson(Object obj) { return MAPPER.writeValueAsString(obj); }`
- **DTO**: `public record UserEvent(String userId, String action, Instant timestamp) {}`
- **Usage**: `UserEvent event = JsonUtil.fromJson(json, UserEvent.class); String json = JsonUtil.toJson(event);`

---

## HTTP Client (`java.net.http.*`)

- **Setup**: `HttpClient client = HttpClient.newBuilder().connectTimeout(Duration.ofSeconds(10)).build();`
- **GET**: `HttpRequest req = HttpRequest.newBuilder().uri(URI.create(url)).timeout(Duration.ofSeconds(20)).GET().build(); HttpResponse<String> resp = client.send(req, HttpResponse.BodyHandlers.ofString());` (check `statusCode() / 100 != 2` for errors)
- **POST**: `HttpRequest req = HttpRequest.newBuilder().uri(URI.create(url)).header("Content-Type", "application/json").POST(HttpRequest.BodyPublishers.ofString(jsonBody)).build();`
- **Headers**: `headers.forEach(builder::header);`
- **Error handling**: Check status code, throw `IntegrationException` on failure

---

## SOAP Client (JAX-WS)

**Note**: Generate strongly-typed stubs from WSDL (`wsimport` or Maven/Gradle plugin), then call port methods.

- **Setup**: `MyService service = new MyService(); MyPort port = service.getMyPort(); BindingProvider bp = (BindingProvider) port;`
- **Endpoint**: `bp.getRequestContext().put(BindingProvider.ENDPOINT_ADDRESS_PROPERTY, endpointUrl);`
- **Timeouts**: `bp.getRequestContext().put("com.sun.xml.ws.connect.timeout", 10_000); bp.getRequestContext().put("com.sun.xml.ws.request.timeout", 20_000);`
- **Headers**: Attach handler chain: `binding.setHandlerChain(chain);` with `SOAPHandler<SOAPMessageContext>` implementation
- **Call**: `MyResponse resp = port.someOperation(req);` (catch `SOAPFaultException`, `WebServiceException`)
- **Header handler**: Implement `handleMessage()` to add headers: `header.addHeaderElement(new QName(name)).addTextNode(value);`

---

## Database Access (`java.sql.*`)

- **Update** (parameterized): `try (PreparedStatement ps = connection.prepareStatement(sql)) { for (int i = 0; i < params.size(); i++) { ps.setObject(i + 1, params.get(i)); } return ps.executeUpdate(); }` (1-based indexing)
- **Query**: `try (PreparedStatement ps = connection.prepareStatement(sql); ResultSet rs = ps.executeQuery()) { while (rs.next()) { Map<String, Object> row = new HashMap<>(); for (int i = 1; i <= meta.getColumnCount(); i++) { row.put(meta.getColumnName(i), rs.getObject(i)); } results.add(row); } }`
- **Transaction**: `connection.setAutoCommit(false); operations.run(); connection.commit();` (catch: `rollback()`, finally: `setAutoCommit(true)`)
- **Note**: Use `PreparedStatement` to prevent SQL injection; connection typically from DataSource/pool

---

## Concurrency Basics (`java.util.concurrent.*`)

- **Executor**: `ExecutorService executor = Executors.newFixedThreadPool(8);`
- **Submit with timeout**: `Future<T> future = executor.submit(task); return future.get(timeout, unit);` (catch `TimeoutException`, cancel future)
- **Parallel**: `List<Future<T>> futures = executor.invokeAll(tasks); for (Future<T> f : futures) { results.add(f.get()); }`
- **Shutdown**: `executor.shutdown(); if (!executor.awaitTermination(60, SECONDS)) { executor.shutdownNow(); }`

---

## Logging

**Note**: Production: SLF4J + Logback/Log4j2. Always log correlation IDs (requestId/eventId).

- **Setup**: `private static final Logger LOG = Logger.getLogger(Service.class.getName());`
- **Logging**: `LOG.info(() -> "message id=" + requestId);`, `LOG.warning(() -> "error id=" + id + " error=" + e.getMessage());`, `LOG.severe(() -> "failed id=" + id);`
- **Pattern**: Log at start/end of operations, include correlation ID, log exceptions with context

---

## Validation

**Fail fast at boundaries**: API input, events, config.

- **Helpers**: `requireNonBlank(String value, String fieldName)`, `requireNonNull(Object value, String fieldName)`, `requirePositive(int value, String fieldName)`, `requireInRange(int value, int min, int max, String fieldName)`
- **Pattern**: `if (value == null || value.isBlank()) { throw new IllegalArgumentException(fieldName + " is required"); }`
- **DTO validation**: `validateUserEvent(UserEvent event) { requireNonBlank(event.userId(), "userId"); requireNonNull(event.timestamp(), "timestamp"); }`

---

## Integration Patterns

**Minimal Integration Handler Skeleton**

- **HTTP flow**: Fetch → Parse → Validate → Persist
  - `String correlationId = UUID.randomUUID().toString(); LOG.info(() -> "HTTP start correlationId=" + correlationId);`
  - `String json = httpClient.getJson(url, Map.of("X-Correlation-Id", correlationId));`
  - `UserEvent event = JsonUtil.fromJson(json, UserEvent.class); ValidationUtil.validateUserEvent(event);`
  - `dbService.executeUpdate("INSERT INTO events (...) VALUES (?, ?, ?)", List.of(...));`
  - `LOG.info(() -> "HTTP done correlationId=" + correlationId);` (catch: log error, rethrow)
- **SOAP flow**: Call → Validate → Persist
  - `MyResponse resp = soapClient.call(req); if (resp.getStatus() == null) { throw new IntegrationException(...); }`
  - `dbService.executeUpdate("INSERT INTO soap_responses (...) VALUES (?, ?)", List.of(...));`
  - Log with correlation ID at start/end/error
