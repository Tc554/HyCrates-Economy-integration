# HyCrates Economy integration
Make [HyCrates](https://builtbybit.com/resources/hycrates.90215/) support your economy plugin!


Making HyCrates support your economy plugin is easy!

Step 1, Make HyCrates an optional dependency in your plugin `manifest.json`:
```json
"OptionalDependencies": {
  "me.TastyCake:HyCrates": "*"
}
```

Step 2, Register it inside your plugin:
```java
// Java
@Override
public void setup() {
    PluginBase plugin = PluginManager.get().getPlugin(new PluginIdentifier("me.TastyCake", "HyCrates"));

    if (plugin != null) {
        Class<?> clazz = plugin.getClass();

        Method method = null;
        for (Method m : clazz.getMethods()) {
            if (m.getName().equals("integrateEconomy") && m.getParameterCount() == 4) {
                method = m;
                break;
            }
        }

        if (method != null) {
            java.util.function.Function<UUID, Double> getBalance = uuid -> {
                // Your method that returns the player's balance
            };

            java.util.function.BiConsumer<UUID, Double> withdraw = (uuid, amount) -> {
                // Your withdraw method
            };

            java.util.function.BiConsumer<UUID, Double> deposit = (uuid, amount) -> {
                // Your deposit method
            };

            try {
                method.setAccessible(true);
                method.invoke(
                    plugin,
                    "YourPluginName",
                    getBalance,
                    withdraw,
                    deposit
                );
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```
```kotlin
// Kotlin
override fun setup() {
    val plugin = PluginManager.get().getPlugin(PluginIdentifier("me.TastyCake", "HyCrates"))

    if (plugin != null) {
        val clazz = plugin::class.java

        val method: Method? = clazz.methods.firstOrNull { m ->
            m.name == "integrateEconomy" && m.parameterCount == 4
        }

        if (method != null) {
            val getBalance = { uuid: UUID -> /* Your method that returns the player's balance (Double) */ }
            val withdraw = { uuid: UUID, amount: Double -> /* Your withdraw method */ }
            val deposit = { uuid: UUID, amount: Double -> /* Your deposit method */ }

            method.isAccessible = true
            method.invoke(
                plugin,
                "YourPluginName",
                getBalance,
                withdraw,
                deposit
            )
        }
    }
}
```

Step 3 (Optional):

Let your users know how to how to make your economy plugin the preferred plugin to use for HyCrates (if they have multiple plugins that integrate with HyCrate, only one of them would be used, so setting up the preferred plugin makes sure they use the correct one)

in HyCrate's `general.json` config file:
```json
"PreferredEconomyPlugin": "YourPluginName"
```
