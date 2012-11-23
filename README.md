# :OpenHVAC
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

### :Home
- :yearlyConsumption :KilowattHour
    - :default 7000
    - :source http://heliostats.org/

### :SaltStorage
- :parts
    - :MoltenSaltStorage --|> :ThermalStorage
        [Molten Salt][http://en.wikipedia.org/wiki/Thermal_energy_storage#Molten_salt_technology]
        - :constraints
            - :cost [:lessThan [:USD 100]]
            - :storage :KilowattHour
            - :volume :MetersCubed
            - __:Heliostat__
                - :source http://heliostats.org/
                - :storage 130
                - :volume 2
            - __:BlueDrum__
                - :volume 0.208197648
        - :prefer
            - :above :Ground
        - :parts
            - _ --|> :SaltTank, _ :nominalTemp [_ :degCelsius]
                - :hotTank, 566
                - :coldTank, 288
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

## :PhaseIIBaseline --|> :MVPBaseline
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

## :GridBaseline --|> :PhaseIIBaseline
- :parts
    - :RobustWirelessNetworking
        [Hinternet?][http://en.wikipedia.org/wiki/High-speed_multimedia_radio]
        - :ports
            - :flow ieee:802dot11