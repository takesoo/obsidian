---
Tags:
  - AWS
  - Docker
---
```
aws ecs execute-command --cluster fujirockwiki \    --task task-id \    --container wordpress \    --interactive \    --command "/bin/bash"
```