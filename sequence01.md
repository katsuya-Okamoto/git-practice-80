```mermaid
sequenceDiagram
    T->>M: make()
    T->>S: make(M)
    T->>M: attach(S)

    activate M
    T->>M: start_working(towers)

    M->>S: notify() → update()
    activate S
    S->>M: pull last_event_name ("work_start")
    Note right of S: "work_start"戦略を実行し、<br/>開始メッセージを表示
    deactivate S

    loop ハノイの塔アルゴリズム
        M->>M: hanoi() recursion
        
        Note over M,Tower: move_disk(source, target)
        M->>Tower: remove()
        Tower-->>M: disk
        M->>Tower: put(disk)

        M->>S: notify() → update()
        activate S
        S->>M: pull last_event_name ("disk_moved")
        S->>M: pull event details (disk, etc.)
        
        Note right of S: "disk_moved"戦略を実行
        S->>M: all_towers()
        M-->>S: tower list
        S->>Tower: out()
        Tower-->>S: display string
        
        deactivate S
    end

    M->>S: notify() → update()
    activate S
    S->>M: pull last_event_name ("work_end")
    Note right of S: "work_end"戦略を実行し、<br/>終了メッセージを表示
    deactivate S

    deactivate M
