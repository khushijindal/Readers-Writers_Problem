# Operating Systems Assignment
# Starve-Free Readers-Writers Problem
#### KHUSHI 18115051
#### Code + Documentation for Starve free Readers-Writers Problem

#### The Readers-Writers Problem is well-known in computer science. A resource can be accessed by readers, who do not modify the resource, and writers, who can modify the resource. When a writer is modifying the resource, no-one else (reader or writer) can access it at the same time, else another writer could corrupt the resource, and another reader could read a partially modified (inconsistent) value. There are three variations possible for the same: which are defined as first, second and third readers-writers problem respectively. The first gives readers the priority and can result in starvation for writers and the second one gives priority to writers and can lead to starvation for readers. The third Readers-Writers Problem deals with giving a Starve-free environment for both readers and writers. The proposed solution for fair starve-free access for readers and writers both is as discussed below.

## Fundamental Concept:
#### Give neither priority: all readers and writers will be granted access to the resource in their order of arrival. If a writer arrives while readers are accessing the resource, it will wait until those readers free the resource, and then modify it. New readers arriving in the meantime will have to wait.
#### The solution thus proposed, uses Semaphores following a First-In-First-Out strategy to provide access control of the resource to either readers or writers.

##
