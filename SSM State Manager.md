- State manager associations continuously enforce configurations on instances, apply the SSM document whenever an instance match the target
- An state manager associations includes 3 components: 
    - SSM Document that defines the state (patch)
    - target (tag query, Resource group, SSM parameter values)
    - Schedule
- The state manager associations applies the configuration as soon as the instance is detected, ensuring the new instances are configured correctly upon launch


Attention:
"desired operating system configurationn, general configuration": can include more than patching, software settings, file configurations

- apply the configuration baseline automatic and persistent when a new instance created