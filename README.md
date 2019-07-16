# servo-statestore
Optune adjust driver that stores adjust state and returns it back on query.

On adjust, this driver saves the state that it receives. On query it takes the state defined in the driver config (lists all components and their settings, including default values), updates the settings values with the last state that it received on adjust (if present) and returns the result.

State is saved/read using an external [library](https://github.com/opsani/servo/blob/master/state_store.py).


# driver configuration
This driver requires configuration that lists the default state of the application - list of components, their settings and default values.

Adjust operations should only set values for settings that are listed in the config, anything else will be accepted but not returned on query.



Sample `config.yaml`:

```
statestore:
  components:                     # required
    web:                          # component name
      settings:
        inst_type:
          type: enum
          unit: ec2
          step: 1
          value: c5.2xlarge       # default value
          values:                 # list values to try
          - c5.2xlarge
          - m5.xlarge
          - m5a.xlarge
        replicas:
          type: range
          min: 1
          max: 50
          step: 1
          unit: count

```
