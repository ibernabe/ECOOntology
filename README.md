# ECO: ontology for computing nodes, software and problems

**Version:** v0.2
**Namespace:** `https://www.ia.urjc.es/ontologies/eco/` (prefix `eco:`)
**Format:** OWL 2 in Turtle syntax
**Imports:** `seas:SystemOntology-1.1` (SEAS)

## Description

ECO models computing nodes (devices), the software running on them, and the problems they can suffer. The problems cover connection failures, data collection failures, errors in physical mechanisms, software malfunctions, devices in critical status, and adverse environmental conditions.

The ontology reuses SEAS concepts such as `seas:FeatureOfInterest`, `seas:System`, `seas:Connection` and `seas:ConnectionPoint`.

## Classes

Main hierarchy:

```
seas:FeatureOfInterest
├── eco:ComputingNode
└── eco:Software
    ├── eco:Application
    ├── eco:Middleware
    └── eco:Service

eco:Problem
├── eco:ConnectionFailed
├── eco:DataCollectionFailed
├── eco:DeviceInCriticalStatus
├── eco:MechanismError
├── eco:ProblemWithEnviromentPhysics
└── eco:SoftwareMalfunction

eco:Event          (no superclass declared)
```

`eco:Application`, `eco:Middleware` and `eco:Service` are pairwise disjoint, and `eco:ComputingNode` is disjoint from `eco:Software`.

## Properties

### Object properties

| Property | Domain | Range | Notes |
| --- | --- | --- | --- |
| `eco:hasProblem` | `eco:ComputingNode`, `eco:Problem` | — | Functional and inverse functional |
| `eco:contains` | — | — | |
| `eco:deployedIn` | — | — | |
| `eco:isComposedOf` | — | — | |
| `eco:localConnectionTo` | — | — | |
| `eco:remoteConnectionTo` | — | — | |
| `eco:related_to` | — | — | |

### Datatype properties

| Domain class | Properties | Range type |
| --- | --- | --- |
| `eco:Problem` | `status`, `timestamp` | `xsd:string`, `xsd:long` |
| `eco:ComputingNode` | `percentageOfDiskUsed`, `connectedToTheNetwork` | — |
| `eco:Software` | `nameSoftware`, `version` | — |
| `eco:Middleware` | `actionsCarriedOut`, `numContainersDeployed`, `numContainersRunning` | — |
| `eco:ConnectionFailed` | `connectedToNetwork`, `discardedPackets`, `droppedPackets`, `packetsReceived`, `packetsSent` | `xsd:boolean`, `xsd:integer`, `xsd:long` |
| `eco:DataCollectionFailed` | `OutOfRangeReading`, `abnormalReading` | `xsd:float`, `xsd:string` |
| `eco:DeviceInCriticalStatus` | `batteryChargeLevel`, `deviceTemperature`, `meanPercentageUseOfCPU`, `percentageOfRAMused`, `freeDiskSpace` | `xsd:integer`, `xsd:long` |
| `eco:MechanismError` | `errorInPhysicalMechanism` | `xsd:boolean` |
| `eco:ProblemWithEnviromentPhysics` | `highLightConditions`, `lowLightConditions` | `xsd:string` |
| `eco:SoftwareMalfunction` | `messageErrorSoftware`, `softwareIsRunnig` | `xsd:string`, `xsd:boolean` |

Properties with no declared domain: `cpuLoad`, `cpuRequirement`, `date`, `diskSpaceRequirement`, `errorDescription`, `freeRAM`, `id`, `ipAddress`, `networkConnectionTechnology`, `operatingSystem`, `ramMemoryFree`, `ramMemoryInstalled`, `ramRequirement`, `receivedPackets`, `sentPackets`.

## Main axioms

- `eco:ComputingNode ⊑ ∃eco:hasProblem.eco:Problem`
- `eco:Software ⊑ ∃eco:hasProblem.eco:Problem`
- `AllDisjointClasses(eco:Application, eco:Middleware, eco:Service)`
- `eco:ComputingNode ⊓ eco:Software ⊑ ⊥`

## Example

Describing a connection failure:

```turtle
@prefix eco: <https://www.ia.urjc.es/ontologies/eco/> .

eco:fallo1 a eco:ConnectionFailed ;
    eco:packetsSent 1200 ;
    eco:packetsReceived 1150 ;
    eco:droppedPackets 50 ;
    eco:connectedToNetwork false .
```

The `eco:hasProblem` relation is left out of the example because of the open issues listed below.

SPARQL query listing the subclasses of `eco:Problem`:

```sparql
PREFIX eco: <https://www.ia.urjc.es/ontologies/eco/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?tipoProblema
WHERE {
    ?tipoProblema rdfs:subClassOf eco:Problem .
}
```


## Files

- `InstancesExampoleECOv0.2`: example instances.
- `OntologyEcov0.2`: ontology definition.
- `README.md`: this document.

## Authors, base ontology and license

- **Author(s):** Alberto Fernández Gil, Iván Bernabé Sánchez.
- **Based on:** SEAS (Smart Energy Aware Systems) ontology, `https://ci.mines-stetienne.fr/seas/`, imported as `seas:SystemOntology-1.1`.
- **License:** [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0).

## How to cite

- **APA:** Sánchez, I. B., Fernández, A., Billhardt, H., & Ossowski, S. (2023). Problem detection in the edge of IoT applications. *IJIMAI*, 8(3), 85-97.
- **ISO 690:** SÁNCHEZ, Iván Bernabé, et al. Problem detection in the edge of IoT applications. *IJIMAI*, 2023, vol. 8, no 3, p. 85-97.
- **MLA:** Sánchez, Iván Bernabé, et al. "Problem detection in the edge of IoT applications." *IJIMAI* 8.3 (2023): 85-97.
