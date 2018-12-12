# CoffeeApp
Example showing multiple modules for the Coffee App 

## Heater has ...

```java
@ContextModule(name="coffee-heater")
```

## Pump has ...

```java
@ContextModule(name = "coffee-pump", dependsOn = "coffee-heater")
```
... and with Thermosiphon, a Heater is injected and that comes from the heater module

```java
@Singleton
class Thermosiphon implements Pump {

  private final Heater heater;

  Thermosiphon(Heater heater) {
    this.heater = heater;
  }
```
### Main has ...

```java
@ContextModule(name = "coffee-main", dependsOn = {"coffee-pump", "coffee-heater"})
```

... and CoffeeMaker which as dependencies provided from pump and heater

```java
@Singleton
public class CoffeeMaker {

  private final Heater heater;
  private final Pump pump;

  public CoffeeMaker(Heater heater, Pump pump) {
    this.heater = heater;
    this.pump = pump;
  }
  ...
```

## Module ordering

The order that the modules are "wired" in DI is determined based on the `@ContextModule` dependsOn and provides. 
We can see the ordering in the logs:
```console
Running coffee.app.main.CoffeeMakerTest
22:49:07.836 [main] DEBUG io.dinject.BootContext - building context with modules [coffee-heater, coffee-pump, coffee-main]
```

... so we see the `coffee-heater` module is wired first, followed by `coffee-pump` and then `coffee-main`






