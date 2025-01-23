https://docs.docker.com/reference/dockerfile/


### Shell and exec form

The `RUN`, `CMD` and `ENTRYPOINT` instructions all have two possible forms:
- `INSTRUCTION ["executable", "param", "param2"]` (exec form)
- `INSTRUCTION command param1 param2` (shell form)

The exec form makes it possible to avoid triggering shell string munging and to invoke commands using a specific shell. Exec form does not handle string substitution.

The shell form is more relaxed and emphasis ease of use, flexibility and readability