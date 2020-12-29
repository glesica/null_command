# Command(null)

All game state is contained in a `World` object.

Each component of a tower is provided with a reference to the world instance
before it is passed to the tower to have user-defined AI code executed on it.
For example, a sensor internally has access to all game state, but since the
player only accesses the sensor through its public API, the player only has
access to the bits of state that the sensor is designed to provide.

```java
interface Sensor {
    List<Enemy> scan(World w);

    // Possibly other methods like scan(angle, width) or whatever
    // that would yield different results depending on the sensor.
}
```

A tower produces events when its state changes or when it takes some action
(like firing a gun). Perhaps tower components will produce such events as well.
For example, a gun might produce an event when it is ready to be fired.

## Considerations

The simulation aspect of the game must be kept in lock-step to avoid variations
depending on hardware, etc.

We also need a way to make a particular action take up some amount of game time.
For example, a sensor may need a certain amount of "time" to complete a scan.
