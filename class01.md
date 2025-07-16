classDiagram
    direction LR

    subgraph "Application Core"
        THE_TEMPLE
        MONK
        SPOKESPERSON
        TOWER
        DISK
    end

    subgraph "Observer Pattern"
        PUBLISHER
        OBSERVER
    end

    subgraph "Strategy Pattern"
        REPORTING_STRATEGY
        WorkStartStrategy
        WorkEndStrategy
        DiskMovedStrategy
    end

    ' === Class Details ===
    class THE_TEMPLE {
        <<Root>>
        -kuukai: MONK
        -taro: SPOKESPERSON
        -tower_a, tower_b, tower_c: TOWER
        +make()
    }
    class MONK {
        <<Publisher>>
        -tower_A, tower_B, tower_C: detachable TOWER
        +last_event_name: STRING
        +last_event_disk: detachable DISK
        +make()
        +start_working(TOWER, TOWER, TOWER)
        +all_towers(): ITERABLE~TOWER~
        -hanoi(...)
        -move_disk(...)
    }
    class TOWER {
        -disks: LINKED_STACK~DISK~
        +name: CHARACTER
        +count: INTEGER
        +put(DISK)
        +remove(): DISK
        +out: STRING
    }
    class DISK {
        +size: INTEGER
        +is_less(DISK): BOOLEAN
        +out: STRING
    }
    class SPOKESPERSON {
        <<Observer, Context>>
        +monk: MONK
        -strategies: HASH_TABLE
        +make(MONK)
        +update()
    }

    ' === Pattern Details ===
    class PUBLISHER {
        <<Custom>>
        +observers: ARRAYED_LIST~OBSERVER~
        +attach(OBSERVER)
        +notify()
    }
    class OBSERVER {
        <<Library Interface>>
        +update()*
    }
    class REPORTING_STRATEGY {
        <<Interface>>
        +report(SPOKESPERSON)*
    }
    class WorkStartStrategy {
      +report(SPOKESPERSON)
    }
    class WorkEndStrategy {
      +report(SPOKESPERSON)
    }
    class DiskMovedStrategy {
      +report(SPOKESPERSON)
    }

    ' === Relationships ===
    THE_TEMPLE --o MONK : assembles
    THE_TEMPLE --o SPOKESPERSON : assembles
    THE_TEMPLE --o TOWER : assembles
    TOWER o-- "*" DISK : holds
    MONK --> TOWER : uses
    SPOKESPERSON --> MONK : observes (pull)
    MONK --|> PUBLISHER
    SPOKESPERSON --|> OBSERVER
    PUBLISHER o-- OBSERVER
    SPOKESPERSON o-- REPORTING_STRATEGY : uses
    WorkStartStrategy --|> REPORTING_STRATEGY
    WorkEndStrategy --|> REPORTING_STRATEGY
    DiskMovedStrategy --|> REPORTING_STRATEGY
