# Code-Style-Backend-
## 1. Source File Basics



* File encoding: **UTF-8**.

* No unused imports; no wildcard imports (`import java.util.*`).

* One top-level class per file.

* File ordering: license/copyright header (optional), `package`, `import` (one per line, sorted), then the class.

## 2. Naming



| Item              | Rule                   | Example                  |
| ----------------- | ---------------------- | ------------------------ |
| Class / interface | UpperCamelCase         | `CalculationService`     |
| Method / variable | lowerCamelCase         | `toggleFavorite`         |
| Constant          | `UPPER_SNAKE_CASE`     | `DEFAULT_PAGE_SIZE`      |
| Package           | all lowercase          | `com.example.caculate`   |
| Test class        | `<ClassUnderTest>Test` | `CalculationServiceTest` |

## 3. Formatting



* Indentation: **4 spaces**, no tabs.

* Column limit: **100** characters.

* Braces: K\&R style — open brace on the same line, close brace on its own line.



```
if (isValid) {
    doSomething();
} else {
    doOther();
}
```



* One statement per line; semicolons required.

* `@Override` annotation always used when a method overrides a superclass method.

## 4. Statements & Annotations



* `@Autowired` on fields or constructors; prefer constructor injection.

* Keep methods short and single-purpose.

* Prefer `Optional` for return values that may be absent instead of returning `null`.

* Avoid floating-point equality with `==`; use a tolerance or `BigDecimal` where precision matters.

## 5. Javadoc



* Every public class and public method has a Javadoc comment starting with `/**`.

* Use `@param`, `@return`, `@throws` where applicable.



```
/**
 * Converts a numeric string from one base to another.
 *
 * @param value    the number as a string
 * @param fromBase source base (2-36)
 * @param toBase   target base (2-36)
 * @return the number represented in {@code toBase}
 * @throws IllegalArgumentException if the value/base is invalid
 */
public String convertBase(String value, int fromBase, int toBase) { ... }
```

## 6. Spring / REST Conventions in This Project



* Controller classes end with `Controller` and are annotated `@RestController`.

* Service classes end with `Service` and are annotated `@Service`.

* Mapper interfaces are annotated `@Mapper`.

* REST paths use lower-case, hyphen-free resource names: `/api/calculate`, `/api/history/{id}`.

* Every API returns a JSON body containing a boolean `success` field.

* Input validation happens in the Service layer; the Controller only handles HTTP.

## 7. Example (Compliant)



```
@RestController
@RequestMapping("/api")
public class CalcController {

    @Autowired
    private CalculationService calculationService;

    @PostMapping("/calculate")
    public Map<String, Object> calculate(@RequestBody Map<String, String> params) {
        Map<String, Object> response = new HashMap<>();
        try {
            String expression = params.get("expression");
            CalculationHistory history = calculationService.calculateAndSave(expression);
            response.put("success", true);
            response.put("data", history);
        } catch (IllegalArgumentException e) {
            response.put("success", false);
            response.put("error", e.getMessage());
        }
        return response;
    }
}
