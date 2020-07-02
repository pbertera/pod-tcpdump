# pod-tcpdump

`pod-tcpdump` is an `oc` plugin implemented in bash which is useful to gather a tcpdump on a running pod.

## Install

You can install from source:

```
$ curl -so oc-pod_tcpdump https://raw.githubusercontent.com/pbertera/pod-tcpdump/master/pod-tcpdump
$ sudo install oc-pod_tcpdump /usr/local/bin
```

## Usage

```
$ oc pod tcpdump POD [-n NAMESPACE] [-f FILENAME] [-- TCPDUMP-OPTIONS]
```
```
$ oc pod-tcpdump -h
pod-tcpdump is a debugging tool helping to start a tcpdump on a running pod.
Tcpdump is executed with the default options '-s0 -U -w -'. Be aware that passing the '-w file.pcap' option will not work: tcpdump is
executed in a container, file.pcap will be not available on the local machine. In case you want to use some tcpdump option and same the
data on a file you should use -f and the tcpdump '-w -' option 'oc pod-tcpdump -f dump.pcap -- -w - [..]'

Usage:
    oc pod-tcpdump POD [-n NAMESPACE] [-f OUT-FILE] -- [TCPDUMP-OPTIONS]

Example:
    Start a tcpdump on the pod nginx-1-7xm8j in the current namespace:
    oc pod-tcpdump nginx-1-7xm8j 

    Start a tcpdump on the pod in the openshift-ingress pod:
    oc pod-tcpdump -n openshift-ingress 444444

    Start a tcpdump on the pod nginx-1-7xm8j with custom options:
    oc pod-tcpdump nginx-1-7xm8j -- -i tun0 -nn -s0

    Start a tcpdump on the pod and save the output on a specific file (by default the filename is auto-generated)
    oc pod-tcpdump -f dump.pcap nginx-1-7xm8j

    Start a tcpdump on the pod and print the output on standard output
    oc pod-tcpdump -f - nginx-1-7xm8j
```

[![asciicast](https://asciinema.org/a/UXuc66hM4kCTZVK5Tc9gsxuoZ.svg)](https://asciinema.org/a/UXuc66hM4kCTZVK5Tc9gsxuoZ)
