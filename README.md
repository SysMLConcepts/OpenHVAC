# :OpenHVAC
low impact space conditioning appliance

- :maximize
    :OverallFeasibility
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
    - :DevelopmentCost
    - :ManufacturingCost
    - :Complexity


## :ConditionedSpace
- :properties
    - :yearlyConsumption :KilowattHour
    - :area :MetersSquared

### windows: :Window

## :Home --|> :ConditionedSpace
- __:HeliostatHome__
    - :yearlyConsumption 7000
        - :source http://heliostats.org/

## :Baseline
### :ThermalStorage
- :properties
    - :storage :KilowattHour
    - :volume :MetersCubed
    - :cost :USD
- :ports
    - _ :flow :TemperatureFluid 
        - :hotFluid
        - :coldFluid
- :prefer
    - :above :Ground
###: ControlSystem
- :powerDraw :Watts

## :MVPBaseline --|> :Baseline
### :MoltenSaltStorage --|> :ThermalStorage
[Molten Salt][http://en.wikipedia.org/wiki/Thermal_energy_storage#Molten_salt_technology]

- __:Heliostat__
    - _:source_ http://heliostats.org/
    - _:storage_ 130
    - _:volume_ 2
- :parts
    - _ --|> :SaltTank, _ :nominalTemp [_ :degCelsius]
        - :hotTank, 566
        - :coldTank, 288
    
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
            
## :Kickstart --|> ks:KickstarterBusinessModel
- :parts
    - :rewards
       - :DataPointAward --|> ks:Reward
           Your home/office can be one of the design points
       - :StickerReward --|> ks:DataPointAward
           .. and a printed sticker
       - :PlansCDReward --|> :SitckerReward
           .. and a DVD containing the plans and instructional video
       - :KitReward --|> :KitReward
           ... and unassembled parts
       - :PreorderDownpaymentGambol
           ... and preorder of finished product (or donation)
       - :FreeBeerReward --|> :PreorderDownpaymentGambol
           ... a visit to our fab facility and a free beer 
       - :PrototypeReward --|> :PlantTourReward
           ... a (potentially working) test artifact
           - :limitTo :NumberTestArtifacts
           

## :HouseCrowdsourcingSet --|> cf:CrowdFlowerDataSet
- :parts
    - :questions
        - :SpaceLocation --|> cf:Question
            What is the latitude and longitude of the home, office or other 
            space where you'd like to use OpenHVAC?
            - :type :GeoPoint
        - :SpaceArea --|> cf:Question
            What is the square meterage of your space?
            - :type :MetersSquared
        - :AnnualPowerBill --|> cf:Question
            What is the annual power bill for your location?
            - :type :USD
        - :InsulationValue --|> cf:Question
            What is the approximate R-Value for your space?
            - :type leed:RValue