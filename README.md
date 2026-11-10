# ECO: ontología de nodos de computación, software y problemas

**Versión:** v0.2 **Namespace:** `https://www.ia.urjc.es/ontologies/eco/` (prefijo `eco:`) **Formato:** OWL 2 en sintaxis Turtle **Importa:** `seas:SystemOntology-1.1` (SEAS)

## Descripción

ECO modela los nodos de computación (dispositivos), el software que se ejecuta en ellos y los problemas que pueden sufrir. Los problemas cubren fallos de conexión, fallos en la recogida de datos, errores en mecanismos físicos, fallos de software, dispositivos en estado crítico y condiciones ambientales adversas.

La ontología reutiliza conceptos de SEAS, como `seas:FeatureOfInterest`, `seas:System`, `seas:Connection` y `seas:ConnectionPoint`.

## Clases

Jerarquía principal:

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

eco:Event          (sin superclase declarada)
```

`eco:Application`, `eco:Middleware` y `eco:Service` son disjuntas entre sí, y `eco:ComputingNode` es disjunta de `eco:Software`.

## Propiedades

### De objeto

| Propiedad                | Dominio                            | Rango | Observaciones                 |
| ------------------------ | ---------------------------------- | ----- | ----------------------------- |
| `eco:hasProblem`         | `eco:ComputingNode`, `eco:Problem` | —     | Funcional e inversa funcional |
| `eco:contains`           | —                                  | —     |                               |
| `eco:deployedIn`         | —                                  | —     |                               |
| `eco:isComposedOf`       | —                                  | —     |                               |
| `eco:localConnectionTo`  | —                                  | —     |                               |
| `eco:remoteConnectionTo` | —                                  | —     |                               |
| `eco:related_to`         | —                                  | —     |                               |

### De dato

| Clase de dominio                   | Propiedades                                                                                                 | Tipo de rango                            |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| `eco:Problem`                      | `status`, `timestamp`                                                                                       | `xsd:string`, `xsd:long`                 |
| `eco:ComputingNode`                | `percentageOfDiskUsed`, `connectedToTheNetwork`                                                             | —                                        |
| `eco:Software`                     | `nameSoftware`, `version`                                                                                   | —                                        |
| `eco:Middleware`                   | `actionsCarriedOut`, `numContainersDeployed`, `numContainersRunning`                                        | —                                        |
| `eco:ConnectionFailed`             | `connectedToNetwork`, `discardedPackets`, `droppedPackets`, `packetsReceived`, `packetsSent`                | `xsd:boolean`, `xsd:integer`, `xsd:long` |
| `eco:DataCollectionFailed`         | `OutOfRangeReading`, `abnormalReading`                                                                      | `xsd:float`, `xsd:string`                |
| `eco:DeviceInCriticalStatus`       | `batteryChargeLevel`, `deviceTemperature`, `meanPercentageUseOfCPU`, `percentageOfRAMused`, `freeDiskSpace` | `xsd:integer`, `xsd:long`                |
| `eco:MechanismError`               | `errorInPhysicalMechanism`                                                                                  | `xsd:boolean`                            |
| `eco:ProblemWithEnviromentPhysics` | `highLightConditions`, `lowLightConditions`                                                                 | `xsd:string`                             |
| `eco:SoftwareMalfunction`          | `messageErrorSoftware`, `softwareIsRunnig`                                                                  | `xsd:string`, `xsd:boolean`              |

Propiedades sin dominio declarado: `cpuLoad`, `cpuRequirement`, `date`, `diskSpaceRequirement`, `errorDescription`, `freeRAM`, `id`, `ipAddress`, `networkConnectionTechnology`, `operatingSystem`, `ramMemoryFree`, `ramMemoryInstalled`, `ramRequirement`, `receivedPackets`, `sentPackets`.

## Axiomas principales

* `eco:ComputingNode ⊑ ∃eco:hasProblem.eco:Problem`
* `eco:Software ⊑ ∃eco:hasProblem.eco:Problem`
* `AllDisjointClasses(eco:Application, eco:Middleware, eco:Service)`
* `eco:ComputingNode ⊓ eco:Software ⊑ ⊥`

## Ejemplo

Ejemplo de descripción de un fallo de conexión:

```turtle
@prefix eco: <https://www.ia.urjc.es/ontologies/eco/> .

eco:fallo1 a eco:ConnectionFailed ;
    eco:packetsSent 1200 ;
    eco:packetsReceived 1150 ;
    eco:droppedPackets 50 ;
    eco:connectedToNetwork false .
```

La relación `eco:hasProblem` no aparece en el ejemplo porque tiene los problemas indicados en la sección siguiente.

Consulta SPARQL para listar las subclases de `eco:Problem`:

```sparql
PREFIX eco: <https://www.ia.urjc.es/ontologies/eco/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?tipoProblema
WHERE {
    ?tipoProblema rdfs:subClassOf eco:Problem .
}
```



## Autoría y licencia

* **Autor/es:** : Alberto Fernández Gil, Iván Bernabé Sánchez
* **Licencia:** : Licencia CC BY 4.0

