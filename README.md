# :LISCA
low impact space conditioning appliance

- :maximize
    - :sumProduct
        - [:HeatMaxDelta :WeightEnvironmentCold]
        - [:CoolMaxDelta :WeightEnvironmentHot]
        - [:MeatTimeBetweenFailure :WeightReliability]
        - [:ProportionHackerMaterial :WeightHackability]
        - [:CoolCubicMeterAirSpeed :WeightEnvironmentHot]
        - [:HeatCubicMeterAirSpeed :WeightEnvironmentCold]
        - [:NetYearlyPowerGenerated :WeightAutarch]
    - :divideBy
        - :product [:LifecycleOwnershipCost :CostWeight]
- :minimize
    - :LifecycleOwnershipCost


## :MVPBaseline --|> :Baseline
### :SaltStorage
- :parts
    - :NaClTemperatureGradientStorage
        - :constraints
            - :cost [:lessThan [:USD 100]]
        - :prefer
            - :above :Ground
- :ports
    - _ :flow :TemperatureFluid 
        - :hotFluid
        - :coldFluid
    
### :WindowUnit
- :parts
    - :VortexTube
        [Ranque-Hilsch vortex tube][http://en.wikipedia.org/wiki/Vortex_tube]
    - :ports
        - _ :flow :TemperatureFluid
            - :hotFluid
            - :coldFluid
        - _ :flow :EnvironmentAir
            - :hotAir
            - :coldAir

## :PhaseIIBaseline --|> :Baseline
- :parts
    - :PyMCU --|> :Microcontroller
        [Python-Conrolled Microcontroller][http://pymcu.com/]
        - :cost [:USD 24.95]
    - :BatterySystem
        Some kind of power tool replaceable battery set: probably not going to 
        be :openSource
        
        - :ports
            - :flow :alternatingCurrent
            - :flow :USB
    - :PhotovoltaicArray
        - :flow :alternatingCurrent

## :GridBaseline --|> :Baseline
- :parts
    - :RobustWirelessNetworking
        [Hinternet?][http://en.wikipedia.org/wiki/High-speed_multimedia_radio]
        - :ports
            - :flow ieee:802dot11