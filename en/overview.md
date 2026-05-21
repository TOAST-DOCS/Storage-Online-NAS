## Storage > NAS > Overview

NAS offers easy data sharing by attaching shared storage to instances.

<a id="features"></a>
## Features

<a id="features.sharing"></a>
### Sharing

You can mount volume on one or more instances for use.
Supported protocols: NFS v3 (Linux), CIFS(Windows)

<a id="features.convenient"></a>
### Convenient

You can mount file-level storage through a network file system without additional file system configuration and perform file-level operations.

<a id="features.flexible"></a>
### Flexible

Volume capacity can be scaled up and down even while in use.

<a id="features.secure"></a>
### Secure  

Volume is isolated from other projects' networks because it is accessed through a project's network.
You can keep your data safe by encrypting it with the XTS-AES-256 algorithm.

<a id="glossary"></a>
## Glossary

<a id="glossary.NAS"></a>
### NAS (network-attached storage)

NAS is a file-based storage device accessible through a network. NAS volumes can be mounted like a local disk to store or retrieve files, and can also be used for data sharing between multiple servers. Basic security features such as access control and authentication are also provided.

<a id="glossary.volume"></a>
### Volume

A volume is a logical storage space of NAS where data can be stored or read by mounting it on instances.

<a id="glossary.snapshots"></a>
### Snapshots

A snapshot is a read-only copy of a volume used to back up and restore data at a specific point in time.
Users can specify the automatic creation time once per day, and data can be restored to that state through the created snapshot.
Since snapshots consume storage space in the volume, snapshot creation can be disabled if not needed.