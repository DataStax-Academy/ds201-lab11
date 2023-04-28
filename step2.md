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
 <a href='command:katapod.loadPage?[{"step":"step2"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 2 of 2</span>
 <a href='command:katapod.loadPage?[{"step":"finish"}]' 
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>

</div>

<!-- CONTENT -->

<div class="step-title">Configure the nodes</div>

✅ Start *cqlsh*
```
./node1/bin/cqlsh
```

✅ Execute the following statements to create a keyspace with the *NetworkTopologyStrategy* that will store one replica per data center:

```cql
CREATE KEYSPACE killrvideo WITH replication = {
  'class': 'NetworkTopologyStrategy', 
  'dc-seattle': 1,'dc-atlanta': 1
};
```

✅ Switch to the *killrvideo* keyspace:

<details class="katapod-details">
  <summary>Solution</summary>

```cql
USE killrvideo;
```

</details>
<br>

✅ Execute the following commands to re-create the *videos_by_tag* table and re-import the data:

```cql
CREATE TABLE videos_by_tag (
    tag text,
    video_id uuid,
    added_date timestamp,
    title text,
    PRIMARY KEY ((tag), added_date, video_id))
    WITH CLUSTERING ORDER BY (added_date DESC, video_id ASC);

COPY videos_by_tag(tag, video_id, added_date, title)
FROM './data-files/videos-by-tag.csv'
WITH HEADER=TRUE;
```

✅ Exit *cqlsh*
```
QUIT
```

✅ Execute the following commands to determine which nodes the partition replicas reside on:

```cql
./node1/bin/nodetool getendpoints killrvideo videos_by_tag 'cassandra'

./node1/bin/nodetool getendpoints killrvideo videos_by_tag 'datastax'
```

You’ll be able to see the replicas nodes, represented by their IP addresses.

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"step2"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
  <a href='command:katapod.loadPage?[{"step":"finish"}]' 
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>

</div>
