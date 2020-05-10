类

```java
class Emploee
{
  private String name;
  private double salary;
  private LocalDate hireDate;
  
  public Emploee(String n, double s, int year, int month, iny day) {
    name = n;
    salary = s;
    hireDate = LocalDate.of(year, month, day);
  }
  
  public String getName() {
    return name;
  }
}
```

类之间，最常见的关系有
- 依赖 （uses-a）
- 聚合 （has-a）
- 继承 （is-a）
