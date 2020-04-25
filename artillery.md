---
description: >-
  a modern, powerful & easy-to-use solution for load testing and functional
  testing
---

# Artillery

## Resources

* Basics: [https://artillery.io/docs/basic-concepts/](https://artillery.io/docs/basic-concepts/)
* Docs: [https://artillery.io/docs/](https://artillery.io/docs/)

```bash
npm i -g artillery
```

```bash
artillery quick --count 10 -n 20 https://artillery.io/
```

This command will create 10 "virtual users" each of which will send 20 HTTP `GET` requests to `https://artillery.io/`.

To write scenarios

```yaml
config:
  target: 'http://localhost:8050/api/v1/metadata'
  phases:
    - duration: 60
      arrivalRate: 20
  defaults:
    headers:
      Execution-Context: ''
scenarios:
  - flow:
    - get:
        url: "/9dc91b46-76da-40d6-a884-3f71756adfe6"
    - get:
        url: "/9ba6c7b1-64e6-4fbd-8316-ae3285b32f9f"
    - get:
        url: "/ca7068e3-7a95-4d3a-a3e4-08dac9ad345b"

```



