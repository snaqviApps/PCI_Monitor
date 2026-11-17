graph TB
    %% Master Repository Container
    subgraph MasterRepo ["Master Repository: PCI-Monitor"]
        direction TB

        subgraph Submodule1 ["Submodule 1: pci-backend"]
            direction TB
            cpp_lib["C++ Library (libpci_diag.so)"]
            sysfs_layer["Linux Kernel /sysfs"]
            grpc_server["gRPC C++ Server / Daemon"]
            
            sysfs_layer -->|Reads devices| cpp_lib
            cpp_lib -->|Wraps telemetry| grpc_server
        end

        subgraph Submodule2 ["Submodule 2: pci-kmp-ui"]
            direction TB
            kmp_client["KMP UI Core (Compose Multiplatform)"]
            grpc_android_client["gRPC Android Client (Kotlin)"]
            
            grpc_android_client -->|Emits Flow| kmp_client
        end
    end

    %% Network / IPC Transport Layer
    grpc_android_client ==>|gRPC Stream over TCP/IP (Proto Buffers)| grpc_server

    %% Target Hardware & Runtime Environments
    subgraph TargetUbuntu ["Target Environment: Ubuntu Host"]
        ubuntu_os[Ubuntu OS & Kernel]
        ubuntu_daemon[gRPC Server Daemon]
        ubuntu_so[libpci_diag.so]

        ubuntu_os -.- ubuntu_daemon
        ubuntu_daemon -.- ubuntu_so
    end

    subgraph TargetAndroid ["Target Environment: Android Mobile / Desktop"]
        android_app[Android App UI]
    end

    %% Deployment & Runtime Binding
    grpc_server -.->|Runs inside| ubuntu_daemon
    kmp_client -.->|Runs inside| android_app

    %% Color Styling
    classDef master fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px;
    classDef sub fill:#e3f2fd,stroke:#1565c0,stroke-width:2px;
    classDef transport fill:#fff9c4,stroke:#fbc02d,stroke-width:2px;
    classDef target fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;

    class MasterRepo master;
    class Submodule1,Submodule2 sub;
    class grpc_android_client transport;
    class TargetUbuntu,TargetAndroid target;
