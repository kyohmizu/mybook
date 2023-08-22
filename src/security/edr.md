# EDR

https://www.form3.tech/engineering/content/bypassing-ebpf-tools

## Tools

- ISM CloudOne
  - https://ismcloudone.com/function/behavior/
- CrowdStrike falcon
  - https://www.crowdstrike.com/products/
- sysdig falco
  - https://sysdig.com/blog/sysdig-edr-container-kubernetes/
- Cilium Tetragon
  - https://github.com/cilium/tetragon

## CrowdStrike falcon

https://github.com/pe3zx/crowdstrike-falcon-queries

https://www.reddit.com/r/crowdstrike/comments/s671jh/how_to_search_only_for_events_for_certain_tags/

```
TargetFileName != "" event_platform=Lin event_simpleName=CriticalFileModified | lookup local=true aid_master aid OUTPUT SensorGroupingTags | search SensorGroupingTags="*prd*"
```
