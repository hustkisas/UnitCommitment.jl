<h1 align="center">UnitCommitment.jl</h1>
<p align="center">
  <a href="https://github.com/ANL-CEEESA/UnitCommitment.jl/actions?query=workflow%3ATest+branch%3Adev">
    <img src="https://github.com/iSoron/UnitCommitment.jl/workflows/Tests/badge.svg"></img>
  </a>
  <a href="https://doi.org/10.5281/zenodo.4269874">
    <img src="https://zenodo.org/badge/doi/10.5281/zenodo.4269874.svg" alt="DOI"></img>
  </a>
  <a href="https://github.com/ANL-CEEESA/UnitCommitment.jl/releases/">
    <img src="https://img.shields.io/github/v/release/ANL-CEEESA/UnitCommitment.jl?include_prereleases&label=pre-release">
  </a>
  <a href="https://github.com/ANL-CEEESA/UnitCommitment.jl/discussions">
    <img src="https://img.shields.io/badge/GitHub-Discussions-%23fc4ebc" />
  </a>
</p>

**UnitCommitment.jl** (UC.jl) is an optimization package for the Security-Constrained Unit Commitment Problem (SCUC), a fundamental optimization problem in power systems used, for example, to clear the day-ahead electricity markets. The package provides benchmark instances for the problem and Julia/JuMP implementations of state-of-the-art mixed-integer programming formulations.


## Power Systems and Unit Commitment 
Unit commitment (UC) is essentially to schedule energy production of energy generation units in an electrcially connected system to meet energy demand in every time instance in a most cost-effective way, while satisfying security and reliability requirements. Some variants of UC include security constraints, called security-constrained UC (or SCUC). In this package, UC specifically refers to SCUC. 
### Power systems
A power system is a set of energy components that electrically connected, which mostly composes of generation side, demand side, and the the transmission network that connect the two sides. Figure 1 shows a power system on a conceptual level. 
<center>
    <img src="./docs/src/assets/powersystem.png" style="max-width: 100px"/>
    <div><b>Figure 1.</b> Power system on a conceptual level.</div>
    <br/>
</center>

* **Generation** The generation side consists of various energy generating units include nuclear generators, thermal generators, hydropower generators, renewable generators (e.g., wind farms, solar farms, marine energy generators, etc.), etc. Energy storages (e.g., pumphydro, batteries, fly wheels, etc.) are also considered on the generation side.
* **Demand** The demand side traditionally refers to the electricity comsummed by all sorts of electricity costumers, which is also often called as 'electricity load' or 'load'. In recent years, significant amounts of distributed energy resources (DERs) have been integrated into the grid, mostly behind the meter such as roof-top solar PV systems. Demand/load is not simply electricity comsumption anymore. In some days, DERs produce excessive electricity and demand/load becomes a negative number, indicating they supply power back to the grid.
* **Transmission network** The generation and demand sides are connected by electricity delivery network, such as transmission lines and transformers, so that energy can flow from one place to another. The electricity delivery network consists of transmission networks (higher-voltage networks that move energy from one region to another) and distribution network. (lower-voltage network thats distribute energy locally to customers). In transmission system analysis (such as UC), a distribution network is usually aggregated into a single node in the transmission network. 

Note that power systems also have many other components such as protection, measurement, control, communication system, etc., which are not considered in the conceptual illustraion above and UC. Fundemental physics that govern power systems include [Kirchhoff's laws], [Dynamical Theory of the Electromagnetic Field], and [Electromechanics].

[Kirchhoff's laws]: https://en.wikipedia.org/wiki/Kirchhoff%27s_circuit_laws
[Dynamical Theory of the Electromagnetic Field]: https://en.wikipedia.org/wiki/A_Dynamical_Theory_of_the_Electromagnetic_Field
[Electromechanics]: https://en.wikipedia.org/wiki/Electromechanics

### Unit commitment
UC is the decision to determine the scheduling of energy generation components to meet electricity demand while satisfying security requirements. A unit refers to a energy generating component. Although there are various types of energy generating components, their operations can be described generally as (1) whether the component should be turned on and (2) how much power output it should produce. The two operations are the major decisions in a UC. The various units also share common operational charateristics, such as minimal or maximal output limits and ramping limits (how fast it can change its power output). 

The objective of UC is usually minimizing the cost of production. The costs include energy production cost, i.e., the fuel burned to produce energy, and the startup cost, i.e., reheating and extra maintenance to mitigate the effects of thermal stress, which could depend on how long the generator has been offline. 

The constraints of UC consists of physical operation limits of a generator, generation-demand balance, and security requirements. Physical operation limits include min/max output levels, minimum up/down time, ramping limits, etc. Generation-demand balance requires the energy produced at each time interval must equal demand. Security requirements include transmission line thermal limits, N-1 security constraints, contingency reserves, etc. 

The most popular approach used by industry nowadays to solve a UC is to model it as a mixed-integer linear programming problem and solve it with commercial MIP solvers. 

### Applications of UC

UC is one of the fundemental optimization problems in power systems and has been widely used in operations, planning, and electricity markets. 

In electricity markets, bid offers for producing and purchasing electricity are submitted to independent system operators (ISOs) every night. ISOs then solve a day-ahead UC within a time window, usually 30 minutes, and use the solution to determine which generators will be committed and the price of electricity. In real time, ISOs solve UC every couple of minutes (e.g., 15 minutes) to determine generator commitment and real-time energy pricing. Considering the $400 billion electricity market size annually in U.S. alone, UC is inarguably one of the most important problems in power sector. 

In system expansion planning, e.g., constructing a new transmission line or installing new generators, UCs are solved under various future scenarios with different load forecasting and renewable integration levels to analyze econoimc and reliability impacts. 


## Package Components

* **Data Format:** The package proposes an extensible and fully-documented JSON-based data format for SCUC, developed in collaboration with Independent System Operators (ISOs), which describes the most important aspects of the problem. The format supports the most common generator characteristics (including ramping, piecewise-linear production cost curves and time-dependent startup costs), as well as operating reserves, price-sensitive loads, transmission networks and contingencies.
* **Benchmark Instances:** The package provides a diverse collection of large-scale benchmark instances collected from the literature, converted into a common data format, and extended using data-driven methods to make them more challenging and realistic.
* **Model Implementation**: The package provides Julia/JuMP implementations of state-of-the-art formulations and solution methods for SCUC, including multiple ramping formulations ([ArrCon2000][ArrCon2000], [MorLatRam2013][MorLatRam2013], [DamKucRajAta2016][DamKucRajAta2016], [PanGua2016][PanGua2016]), multiple piecewise-linear costs formulations ([Gar1962][Gar1962], [CarArr2006][CarArr2006], [KnuOstWat2018][KnuOstWat2018]) and contingency screening methods ([XavQiuWanThi2019][XavQiuWanThi2019]). Our goal is to keep these implementations up-to-date as new methods are proposed in the literature.
* **Benchmark Tools:** The package provides automated benchmark scripts to accurately evaluate the performance impact of proposed code changes.

[ArrCon2000]: https://doi.org/10.1109/59.871739
[CarArr2006]: https://doi.org/10.1109/TPWRS.2006.876672
[DamKucRajAta2016]: https://doi.org/10.1007/s10107-015-0919-9
[Gar1962]: https://doi.org/10.1109/AIEEPAS.1962.4501405
[KnuOstWat2018]: https://doi.org/10.1109/TPWRS.2017.2783850
[MorLatRam2013]: https://doi.org/10.1109/TPWRS.2013.2251373
[PanGua2016]: https://doi.org/10.1287/opre.2016.1520
[XavQiuWanThi2019]: https://doi.org/10.1109/TPWRS.2019.2892620

## Sample Usage

```julia
using Cbc
using JuMP
using UnitCommitment

import UnitCommitment:
    Formulation,
    KnuOstWat2018,
    MorLatRam2013,
    ShiftFactorsFormulation

# Read benchmark instance
instance = UnitCommitment.read_benchmark(
    "matpower/case118/2017-02-01",
)

# Construct model (using state-of-the-art defaults)
model = UnitCommitment.build_model(
    instance = instance,
    optimizer = Cbc.Optimizer,
)

# Construct model (using customized formulation)
model = UnitCommitment.build_model(
    instance = instance,
    optimizer = Cbc.Optimizer,
    formulation = Formulation(
        pwl_costs = KnuOstWat2018.PwlCosts(),
        ramping = MorLatRam2013.Ramping(),
        startup_costs = MorLatRam2013.StartupCosts(),
        transmission = ShiftFactorsFormulation(
            isf_cutoff = 0.005,
            lodf_cutoff = 0.001,
        ),
    ),
)

# Modify the model (e.g. add custom constraints)
@constraint(
    model,
    model[:is_on]["g3", 1] + model[:is_on]["g4", 1] <= 1,
)

# Solve model
UnitCommitment.optimize!(model)

# Extract solution
solution = UnitCommitment.solution(model)
UnitCommitment.write("/tmp/output.json", solution)
```

## Documentation

1. [Usage](https://anl-ceeesa.github.io/UnitCommitment.jl/0.3/usage/)
2. [Data Format](https://anl-ceeesa.github.io/UnitCommitment.jl/0.3/format/)
3. [Instances](https://anl-ceeesa.github.io/UnitCommitment.jl/0.3/instances/)
4. [JuMP Model](https://anl-ceeesa.github.io/UnitCommitment.jl/0.3/model/)
5. [API Reference](https://anl-ceeesa.github.io/UnitCommitment.jl/0.3/api/)

## Authors
* **Alinson S. Xavier** (Argonne National Laboratory)
* **Aleksandr M. Kazachkov** (University of Florida)
* **Ogün Yurdakul** (Technische Universität Berlin)
* **Feng Qiu** (Argonne National Laboratory)

## Acknowledgments

* We would like to thank **Yonghong Chen** (Midcontinent Independent System Operator), **Feng Pan** (Pacific Northwest National Laboratory) for valuable feedback on early versions of this package.

* Based upon work supported by **Laboratory Directed Research and Development** (LDRD) funding from Argonne National Laboratory, provided by the Director, Office of Science, of the U.S. Department of Energy under Contract No. DE-AC02-06CH11357

* Based upon work supported by the **U.S. Department of Energy Advanced Grid Modeling Program** under Grant DE-OE0000875.

## Citing

If you use UnitCommitment.jl in your research (instances, models or algorithms), we kindly request that you cite the package as follows:

* **Alinson S. Xavier, Aleksandr M. Kazachkov, Ogün Yurdakul, Feng Qiu**. "UnitCommitment.jl: A Julia/JuMP Optimization Package for Security-Constrained Unit Commitment (Version 0.3)". Zenodo (2022). [DOI: 10.5281/zenodo.4269874](https://doi.org/10.5281/zenodo.4269874).

If you use the instances, we additionally request that you cite the original sources, as described in the documentation.

## License

```text
UnitCommitment.jl: A Julia/JuMP Optimization Package for Security-Constrained Unit Commitment
Copyright © 2020-2022, UChicago Argonne, LLC. All Rights Reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted
provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this list of
   conditions and the following disclaimer.
2. Redistributions in binary form must reproduce the above copyright notice, this list of
   conditions and the following disclaimer in the documentation and/or other materials provided
   with the distribution.
3. Neither the name of the copyright holder nor the names of its contributors may be used to
   endorse or promote products derived from this software without specific prior written
   permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR
IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY
AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR
CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
POSSIBILITY OF SUCH DAMAGE.
```

