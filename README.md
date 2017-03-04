# CorrectByConstructionTCFamilies-SoSyM17
This repository documents the resources of the SHARE VM that belongs to the paper *Roland Kluge, Michael Stein, Gergely Varró, Andy Schürr, Matthias Hollick, Max Mühlhäuser: "A Systematic Approach to Constructing Incremental Topology Control Algorithms Using Graph Transformation" (DOI: https://dx.doi.org/10.1007/s10270-017-0587-8)

# VM metadata

* URL: http://is.ieis.tue.nl/staff/pvgorp/share/?page=ConfigureNewSession&vdi=XP-TUe_RK_CorrectByConstructionTC_SoSyM.vdi
* User: sharevm[A}es[D}tu-darmstadt[D}de ([A} = @, [D} = ., [E} = !)
* Passsword: ShareVM[A}RealTime[E}

# FAQs or What can I do with the VM?

## Who can I contact?

If you experience problems, please do not hesistate to write a mail to Roland Kluge (roland[D}kluge[A}es[D}tu-darmstadt[D}de).

## Where should I start?

1. Navigate to the Desktop (*Win+D*)
2. Double-click *Eclipse Neon 1.a*
3. All material is contained in the workspace that opens up.

## Where is the metamodel? Where are the Topology Control rules?

1. In Eclipse, open the project *de.tudarmstadt.maki.tc.cbctc.specification*
2. Double-click the EAP file *de.tudarmstadt.maki.cbctc.specification.eap*
3. Enterprise Architect Lite opens up.
3. The metamodel can be found in the package *de.tudarmstadt.maki.tc.cbctc.model*. 
The Topology Control rules of kTC (and a number of other TC algorithms) can be found in the package *de.tudarmstadt.maki.tc.cbctc.algorithms*.

## How can I run a sample simulation?

1. In eclipse, open the project *simonstrator-simrunner*
2. Navigate to *config/rkluge/launch/"
3. Right-click *GUIRunner-with-EMF.launch* and select *Run as -> GUIRunner-with-EMF*
4. Type the file name *sosym_complete_evaluation.xml* into the search bar.
 * This file contains all parameters of the simulation (e.g., energy model, movement model, protocol stack, overlay application).
5. Press *Start Simulation*
6. Two windows should pop up
 * The *Topology Visualization* window shows the current output WiFi topology 
 * The *Simulation* window shows statistics about the progress of the simulation, simulation speed, etc.
7. Additionally, the log output in the *Console* view of Eclipse provides information about the progress of topology control.

## What do the log messages tell me?
The *Console* view of Eclipse reports on Topology-Control-specific metrics.
An example:
```
23-Nov-2016 17:06:46. INFO 	iter#001 CEs...
23-Nov-2016 17:06:46. INFO 	Adding node Node [id=1, properties={SiS-PHY_LOCATION=PositionVector [13.454846479205782, 73.0], SiS-LOCATION_X=13.454846479205782, SiS-LOCATION_Y=73.0}]
[...]
23-Nov-2016 17:06:46. INFO 	Adding node Node [id=101, properties={SiS-PHY_LOCATION=PositionVector [44.04043792076475, 106.56416757687579], SiS-LOCATION_X=44.04043792076475, SiS-LOCATION_Y=106.56416757687579}]
23-Nov-2016 17:06:46. INFO 	iter#001 CEs   Done (t=247ms, t_check=7ms, numCEs=[+n= 101, -n=   0, +e=1392, -e=   0, mod-d=   0, mod-w=   0, mod-p=   0])
23-Nov-2016 17:06:46. INFO 	iter#001 iTC...(algo=D_KTC(id=1), parameters=Parameters: {a=2.0, coneCount=4.0, k=1.41})
23-Nov-2016 17:06:47. INFO 	iter#001 iTC   Done (t=365ms,  t_check=89ms,  violations=0, allLSMs=1920 [[total count: a=1030, i=624, u= 266, effective counts: a= 790, i=602, u=   0]], effLSMs=1392)
23-Nov-2016 17:06:47. INFO 	iter#001 bTC...(algo=D_KTC(id=1), parameters=Parameters: {a=2.0, coneCount=4.0, k=1.41})
23-Nov-2016 17:06:47. INFO 	iter#001 bTC   Done (t=123ms,  t_check=115ms, violations=0, allLSMs=1392 [[total count: a= 790, i=602, u=   0, effective counts: a= 790, i=602, u=   0]], effLSMs=1392)
23-Nov-2016 17:06:47. INFO 	iter#001 iTC vs. bTC (CE+TC) wrt. [time=4.98 / LSMs=1.00]
23-Nov-2016 17:06:47. INFO 	iter#001 Stat..
23-Nov-2016 17:06:47. INFO 	Total real duration: 0.02min
23-Nov-2016 17:06:47. INFO 	iter#001 Stat  Done (duration=123ms, n=101, m=1812, avgBatPct=71.24, minBatPct=47.09, maxBatPct=96.27, numEmptyNodes=0, numSCCsInInput=1, numSCCsInOutput=1)
23-Nov-2016 17:06:47. INFO 	Simulated Realtime: 0:10:0:0 (H:m:s:ms)
```

**Explanation:**
* ```iter##001``` Denotes the first iteration of the Topology Control algorithm. In this setup, Topology Control is executed periodically every 10min of simulated time.
* ```iter#001 CEs... Adding node Node [...]``` Before running the algorithm, all pending context events are handled.
* ```iter#001 CEs   Done (t=247ms, t_check=7ms, violation=0,...``` At the end of context event handling, the required CPU time for the handling and for the constraint checking is printed. Additionally, the number of detected constraint violations is reported.
* ```iter#001 iTC...(algo=D_KTC(id=1), parameters=Parameters: {a=2.0, coneCount=4.0, k=1.41})``` Then, incremental the topology control algorithm is invoked (here: the traditional kTC with k=1.41). Afterwards, the required CPU time for the algorithm and the constraint checking are printed.
* ```iter#001 bTC...(algo=D_KTC(id=1), parameters=Parameters: {a=2.0, coneCount=4.0, k=1.41})``` For comparison reasons, the batch counterpart of the current algorithm is executed on a copy of the topology and runtime statistics are printed.
* ```iter#001 iTC vs. bTC (CE+TC) wrt. [time=4.98 / LSMs=1.00]``` At the end of a Topology Control run, the costs of the incremental and batch algorithm are compared in terms of CPU time and link state modifications. In this case, the incremental algorithm needed ca. 5 times longer compare to the batch algorithm. The link state modification count is identical in this iteration because the topology is processed for the first time.

## How can I experiment with different algorithms?

Proceed as explained in *How can I run a sample simulation?*, **but** before starting the simulation, exchange the value next to *incrementalAlgorithm* in the table on the left side of the simulation configuration window.

The following algorithms are available. Please use the *name* as value for *incrementalAlgorithm*.
 * Maxpower Topology Control (baseline, name=MAXPOWER_TC)
 * kTC (original, name=D_KTC)
 * Energy-Aware kTC (e-kTC, name=E_KTC)
 * l*-kTC (name=LSTAR_KTC)
 * XTC (name=XTC)
 * Yao (name=Yao)
 * Gabriel Graph (name=GG)
 * Relative Neighborhood Graph (name=RNG)
