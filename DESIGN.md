# Command(null)

Idea: an agent-based tower defense game where towers consist of modular
assemblies and their AI is provided by writing code.

Each tower is equipped based on the resource prioritization specified by the
player. On its turn it can use the resources assigned to it to determine what,
if any, action it should take. In the case of a gun, the action is a firing
solution (angle).

The tower is provided with an opaque blob of state called "the world" which it
can use to find enemies to target, and so on, by passing this state object to
its sensors, guns, etc.

A tower produces events when its state changes or when it takes some action
(like firing a gun). Perhaps tower components will produce such events as well.
For example, a gun might produce an event when it is ready to be fired.

```java
interface Tower {
    void disable();
    
    void enable(World w);

    void provideAmmo(Ammo a);

    void provideCompute(Compute c);

    void provideOperator(Operator o);

    void providePower(Power p);

    void provideSensor(Sensor s);

    void provideWeapon(Weapon w);
}
```

```java
interface Sensor {
    List<Enemy> scan(World w);

    // Possibly other methods like scan(angle, width) or whatever
    // that would yield different results depending on the sensor.
}
```

The simulation aspect of the game must be kept in lock-step to avoid variations
depending on hardware, etc. For this purpose, we have a global "clock". If a
process happens continuously in the context of the simulation itself, then in
reality it occurs discretely and precisely as often as every other process that
occurs continuously within the simulation.

The global clock is also used, indirectly, to remove the effects of hardware
differences by giving processes like sensors a way of taking up a certain amount
of "time".

Each component has a request queue and requires some number of ticks to handle a
given request.

