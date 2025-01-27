
---
Route53

R53 Hosted Zone
- A DNS DB for a domain, e.g. animalsforlife.org. Authoritative for that domain
- Globally resilient
- Created with domain registration via R53 -can be created separately

Zone file
- Hosted by R53 (Public Name Servers)
- Accessible from the public internet and VPCs
- Hosted on 4 public NS for the zone

- Resource Records (RR) created within the Hosted Zone
- Externally Registered domains can point at R53 Public Zone

Private Hosted Zones
- Associated with VPCs and only accessible to the VPC
- Using different accounts is supported via the CLI/API


Split-view Hosted Zones
- Overlapping public and private for PUBLIC and INTERNAL use with the same zone name
- Allows use to created a public hosted zone with the same name as a private hosted zone
- public hosted zone might only have a subset of records as the private hosted zone


CNAME Record
- maps a NAME to another NAM
- Invalid for naked/apex (e.g. catagram.io)

ALIAS Records
- Maps a NAME to an AWS Resource
- Can be used for naked/apex and normal domains
- For non apex/naked - functions like CNAME
- NOTE: default to picking ALIAS for AWS services since they are free when the record points to a AWS resource
- Should be the same "Type" as what the record is pointing at (A name Alias records and CNAME Alias records). If the record that the resource provides is an A record then you need to use an A record ALIAS.


R53 Simple Routing
- Create one record per name
- Each record can have multiple values which are part of that same record
- All values are returned in a random order
- Use when you want to route requests towards one service
- Does not support simple routing
- https://learn.cantrill.io/courses/1231680/lectures/31339800 at 1:25