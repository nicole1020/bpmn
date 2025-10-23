# bpmn
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
    +taskType: enum  // user, service, script, send, receive, manual, callActivity, subProcess
    +loopType: enum? // none, standard, multiInstance
  }

  class Event {
    +eventType: enum  // start, intermediate, end
    +trigger: enum?   // message, timer, error, signal, escalation, link, conditional, terminate, none
    +catchThrow: enum // catch, throw (for intermediate)
  }

  class Gateway {
    +gatewayType: enum // exclusive(XOR), inclusive(OR), parallel(AND), complex, eventBased
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
    +associationType: enum // dataAssociation, annotation
  }

  class TextAnnotation {
    +id: string
    +text: string
  }

  %% Inheritance
  FlowNode <|-- Task
  FlowNode <|-- Event
  FlowNode <|-- Gateway

  %% Containment / membership
  Process "1" o-- "1..*" FlowNode : contains
  Pool "1" o-- "1" Process : encapsulates
  Pool "1" o-- "0..*" Lane
  Lane "1" o-- "0..*" FlowNode : partitions

  %% Control flow
  SequenceFlow "0..*" --> "1" FlowNode : target
  FlowNode "1" --> "0..*" SequenceFlow : outgoing

  %% Message flow (cross-pool only)
  MessageFlow "0..*" --> "1" FlowNode : target
  FlowNode "1" --> "0..*" MessageFlow : outgoing

  %% Data
  DataObject "0..*" <-- Association : data input/output
  DataStore  "0..*" <-- Association
  TextAnnotation "0..*" <-- Association

