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
$ oc pod-tcpdump -h

pod-tcpdump is a debugging tool helping to start a tcpdump on a running pod.
The command starts a debug pod on the node where the pod is scheduled and then executes tcpdump
with nsenter(5) into the pod namespace.

Tcpdump is executed with the default options '-s0 -U -w -'.

Be aware that passing the '-w file.pcap' option will not work: tcpdump is executed in a container, file.pcap will be not available on the local machine.

In case you want to use some tcpdump options and save the data on a file you should use -f and the

tcpdump '-w -' option: 'oc pod-tcpdump -f dump.pcap -- -w - [..]'

If the ++ separator is used instead of -- the subsequent options ar concatenated to the default options (-s0 -U -w -) 

Usage:
    oc pod-tcpdump POD [-n NAMESPACE] [-f OUT-FILE] {--|++} [TCPDUMP-OPTIONS]

Example:
    Start a tcpdump on the pod nginx-1-7xm8j in the current namespace:
    oc pod-tcpdump nginx-1-7xm8j 

    Start a tcpdump on the pod and save the output on a specific file (by default the filename is auto-generated)
    oc pod-tcpdump -f dump.pcap nginx-1-7xm8j

    Start a tcpdump on the pod and print the output on standard output
    oc pod-tcpdump -f - nginx-1-7xm8j

    Start a tcpdump on the pod router-default-7f64869574-c876w in the openshift-ingress pod:
    oc pod-tcpdump -n openshift-ingress router-default-7f64869574-c876w

    List all the available network interfaces on a pod (with tcpdump -D)
    oc pod-tcpdump -n openshift-ingress router-default-7f64869574-c876w -f - -- -D

    Start a tcpdump on the pod router-default-7f64869574-c876w with custom options:
    oc pod-tcpdump -n openshift-ingress router-default-7f64869574-c876w -f dump.pcap -- -i tun0 -s0 -U -w -

    Append some tcpdump options to the default options
    oc pod-tcpdump -n openshift-ingress router-default-7f64869574-c876w -f dump.pcap ++ -i any port 80
```

[![asciicast](https://asciinema.org/a/UXuc66hM4kCTZVK5Tc9gsxuoZ.svg)](https://asciinema.org/a/UXuc66hM4kCTZVK5Tc9gsxuoZ)
