# BPMN Meta-Model (Mermaid Diagram)

```mermaid
classDiagram
  direction LR

  class Process {
    +id: string
    +name: string
    +isExecutable: bool
  }

  class Pool {
    +id: string
    +name: string
  }

  class Lane {
    +id: string
    +name: string
  }

  class FlowNode {
    <<abstract>>
    +id: string
    +name: string
  }

  class Task {
    +taskType: enum
    +loopType: enum?
  }

  class Event {
    +eventType: enum
    +trigger: enum?
    +catchThrow: enum
  }

  class Gateway {
    +gatewayType: enum
    +defaultFlow: SequenceFlow?
  }

  class SequenceFlow {
    +id: string
    +name: string?
    +condition: expr?
    +isDefault: bool
  }

  class MessageFlow {
    +id: string
    +name: string?
  }

  class DataObject {
    +id: string
    +name: string
    +state: string?
  }

  class DataStore {
    +id: string
    +name: string
  }

  class Association {
    +id: string
    +associationType: enum
  }

  class TextAnnotation {
    +id: string
    +text: string
  }

  FlowNode <|-- Task
  FlowNode <|-- Event
  FlowNode <|-- Gateway
  Process "1" o-- "1..*" FlowNode : contains
  Pool "1" o-- "1" Process : encapsulates
  Pool "1" o-- "0..*" Lane
  Lane "1" o-- "0..*" FlowNode : partitions
  SequenceFlow "0..*" --> "1" FlowNode : target
  FlowNode "1" --> "0..*" SequenceFlow : outgoing
  MessageFlow "0..*" --> "1" FlowNode : target
  FlowNode "1" --> "0..*" MessageFlow : outgoing
  DataObject "0..*" <-- Association : data
  DataStore  "0..*" <-- Association
  TextAnnotation "0..*" <-- Association
