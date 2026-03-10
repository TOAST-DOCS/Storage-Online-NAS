## Storage > NAS > Overview

NAS offers easy data sharing by attaching shared storage to instances.


> [Note] 
> The NAS service is only available in Korea (Pangyo) and Korea (Pyeongchon) regions as of March 2023.


<a id="features"></a>
## Features

<a id="features.sharing"></a>
### Sharing

You can mount volume on one or more instances for use.
Supported protocols: NFS v3 (Linux), CIFS(Windows)

<a id="features.convenient"></a>
### Convenient

Mounting of file-level storage removes the need for additional file system configuration.

<a id="features.flexible"></a>
### Flexible

Volume capacity can be scaled up and down even while using in use.

<a id="features.secure"></a>
### Secure  

Volume is isolated from other projects’ networks because it is accessed through a project’s network.
You can keep your data safe by encrypting it with the XTS-AES-256 algorithm.

<a id="glossary"></a>
## Glossary

<a id="glossary.NAS"></a>
### NAS (network-attached storage)

A file-level storage device connected to a computer network that can control access to data from other clients.

<a id="glossary.volume"></a>
### Volume

Logical storage space of NAS that keeps data.
Data can be stored or read by mounting volume on instances.

<a id="glossary.snapshots"></a>
### Snapshots

A read-only copy of volume, which refers to a backup of volume. 
Snapshots allow you to restore data to a point in time. 
In the NAS service, users can specify a point in time to create snapshots automatically once per day.
However, storing snapshots consumes volume space, so it can be disabled if not needed.

