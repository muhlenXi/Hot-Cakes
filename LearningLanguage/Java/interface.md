接口用来描述类具有什么功能。

```java
public interface Comparable
{
  int compareTo(Object other);
}
```

Employee 类实现 Comparable 接口

```java
class Employee implements Comparable<Employee>
{
  public int compareTo(Employee other) {
    return Double.compare(salary, other.salary);
  }
}
```
