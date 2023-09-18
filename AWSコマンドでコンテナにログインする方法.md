---
Tags:
  - AWS
  - Docker
Status: Not started
Last edited time: Invalid date
Created time: Invalid date
---
```
aws ecs execute-command --cluster fujirockwiki \    --task task-id \    --container wordpress \    --interactive \    --command "/bin/bash"
```