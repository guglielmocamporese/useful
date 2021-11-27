# Useful SSH Commands

### Multiple SSH Hops Opening Ports Specified As Input Arg

The goal of these scripts is to reach `server2` from the `local_pc` establishing a SSH connection on a specified `port` passed as argument.

Example of usage:
```bash
# SSH into server2 passing from server1, form the local_pc opening the port 9000
./server2 -p 9000 # local_pc:9000 --> server1:9000 --> server2:9000
```

Put this file into the home of the `local_pc`, and rename it as `.server2`:
```bash
#!/bin/bash

POSITIONAL=()
while [[ $# -gt 0 ]]
do
key="$1"

case $key in
        -p|--port)
        port="$2"
        shift # past argument
        shift # past value
        ;;
esac
done

set -- "${POSITIONAL[@]}" # restore positional parameters

if [ "${port}" != "" ]; then
        echo "Tunnel opened at port ${port}"
        ssh -t -L ${port}:127.0.0.1:${port} user@domain1 "./.server2 -p ${port}"
else
        ssh -t user@domain1 "./.server2"
fi
```

Put this second file into the home of the `server1`, and remane it as `.server2`:
```bash
#!/bin/bash

POSITIONAL=()
while [[ $# -gt 0 ]]
do
key="$1"

case $key in
        -p|--port)
        port="$2"
        shift # past argument
        shift # past value
        ;;
esac
done

set -- "${POSITIONAL[@]}" # restore positional parameters

if [ "${port}" != "" ]; then
        echo "Tunnel opened at port ${port}"
        ssh -L ${port}:127.0.0.1:${port} user@domain2
else
        ssh user@domain2
fi
```
