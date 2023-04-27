<!-- TOP -->
<div class="top">
  <img class="scenario-academy-logo" src="https://datastax-academy.github.io/katapod-shared-assets/images/ds-academy-2023.svg" />
  <div class="scenario-title-section">
    <span class="scenario-title">Replication</span>
    <span class="scenario-subtitle">ℹ️ For technical support, please contact us via <a href="mailto:academy@datastax.com">email</a>.</span>
  </div>
</div>

<!-- NAVIGATION -->
<div id="navigation-top" class="navigation-top">
 <a href='command:katapod.loadPage?[{"step":"intro"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 1 of 2</span>
 <a href='command:katapod.loadPage?[{"step":"step3"}]' 
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Configure the nodes</div>

✅ Open `/workspace/ds201-lab11/node1/conf/cassandra.yaml` in a *nano* or the text editor of your choice and find the `endpoint_snitch` setting:
```
nano /workspace/ds201-lab11/node1/conf/cassandra.yaml
```

The default snitch, *SimpleSnitch* is only appropriate for single datacenter deployments. 

✅ Change the *endpoint_snitch* to *ReplicationingPropertyFileSnitch*, save and close the file.

---
**Note:** *ReplicationingPropertyFileSnitch* should be your go-to snitch for production use.  The rack and datacenter for the local node are defined in *cassandra-rackdc.properties* and propagated to other nodes via Replication.

---

✅ Make the same change to *node2*:
```
nano /workspace/ds201-lab11/node2/conf/cassandra.yaml
```

✅ Open `/workspace/ds201-lab11/node1/conf/cassandra-rackdc.properties` in a *nano* or the text editor of your choice and find the `endpoint_snitch` setting:
```
nano /workspace/ds201-lab11/node1/conf/cassandra-rackdc.properties
```
✅ Set the following values, then close and save the file:

`dc=dc-east`<br>
`rack=rack-red`


This is the file that the *ReplicationingPropertyFileSnitch* uses to determine the rack and data center this particular node belongs to.

Racks and datacenters are purely logical assignments to Cassandra. You will want to ensure that your logical racks and data centers align with your physical failure zones.


✅ Open `/workspace/ds201-lab11/node2/conf/cassandra-rackdc.properties`:
```
nano /workspace/ds201-lab11/node2/conf/cassandra-rackdc.properties
```
✅ Set the following values, then close and save the file:

`dc=dc-west`<br>
`rack=rack-red`

Although the rack names for the two nodes are the same, each rack lives independently in a different data center (dc-east vs. dc-west).

✅ Start *node1*:
```
/workspace/ds201-lab11/node1/bin/cassandra
```

Wait for *node1* to start.

✅ Start *node2*:
```
/workspace/ds201-lab11/node2/bin/cassandra
```

✅ Check on the cluster status:
```
/workspace/ds201-lab11/node2/bin/nodetool status
```
You should now see that the nodes are in different datacenters.

<img src="https://katapod-file-store.s3.us-west-1.amazonaws.com/ds201/lab11-image01.png" />

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"intro"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
  <a href='command:katapod.loadPage?[{"step":"step3"}]' 
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>
