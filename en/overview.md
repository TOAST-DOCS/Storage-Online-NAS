## Storage > NAS > Overview

NAS offers easy data sharing by attaching shared storage to instances.


> [Note] 
> The NAS service is only available in Korea (Pangyo) and Korea (Pyeongchon) regions as of March 2023.


## Features

### Sharing

You can mount NAS storage on one or more instances for use.
Supported protocols: NFS v3 (Linux)

### Convenient

Mounting of file-level storage removes the need for additional file system configuration.

### Flexible

Storage capacity can be scaled up and down even while using in use.

### Secure  

NAS storage is isolated from other projects’ networks because it is accessed through a project’s network.


## Glossary

### NAS (network-attached storage)

A file-level storage device connected to a computer network that can control access to data from other clients.

### Storage

Logical storage space of NAS that keeps data.  
Data can be stored or read by mounting NAS storage on instances.


### Snapshots

A read-only copy of NAS storage, which refers to a backup of NAS storage. 
Snapshots allow you to restore data to a point in time. 
In the NAS service, users can specify a point in time to create snapshots automatically once per day.
However, storing snapshots consumes NAS storage space, so it can be disabled if not needed.

