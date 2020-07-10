# pod-tcpdump

`pod-tcpdump` is an `oc` plugin implemented in bash which is useful to gather a tcpdump on a running pod.

This plugin requires the `oc` CLI and works on OpenShift, cannot be used on a vanilla Kubernetes.

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
The command starts a debug pod on the node where the pod is scheduled and
then executes tcpdump with nsenter(1) into the pod namespace.

Usage:
    oc pod-tcpdump POD [OPTIONS]

Options:
    -q | --quiet : run oc pod-tcpdump in quiet mode, without printing any message,
                   useful with -f - and to pipe the output directly to wireshark

    -n | --namespace NS : define the namespace where the POD is residing,
                          if not defined uses the current namespace

    -f | --filename FILE : the filename where the output of tcpdump should be
                           saved, using -f - will print on stdout

    -- | ++ TCPDUMP-OPTIONS : a separator between the oc pod-tcpdump options and the
                              tcpdump options. Using ++ the TCPDUMP-OPTIONS
                              are appended to the defaults (-s0 -U -w -).
                              Using -- the default options are removed

Remember that passing the '-w file.pcap' option will not work: tcpdump is
executed in a container, file.pcap will be not available on the local machine.

Example:
    Start a tcpdump on the pod nginx-1-7xm8j in the current namespace:
    $ oc pod-tcpdump nginx-1-7xm8j 

    Start a tcpdump on the pod and save the output on a specific file
    (by default the filename is auto-generated)
    $ oc pod-tcpdump -f dump.pcap nginx-1-7xm8j

    Start a tcpdump on the pod and print the output on standard output
    $ oc pod-tcpdump -f - nginx-1-7xm8j

    Start a tcpdump on the pod router-c876w in the openshift-ingress pod:
    $ oc pod-tcpdump -n openshift-ingress router-c876w

    List all the available network interfaces on a pod (with tcpdump -D)
    $ oc pod-tcpdump router-c876w -f - -- -D

    Start a tcpdump on the pod router-c876w with custom options:
    $ oc pod-tcpdump router-c876w -f dump.pcap -- -i tun0 -s0 -U -w -

    Append some tcpdump options to the default options
    $ oc pod-tcpdump router-c876w -f dump.pcap ++ -i any port 80

    Start the tcpdump in quiet mode and pipe to tshark
    $ oc pod-tcpdump ruby-ex-1-wbn4l -f - -q | tshark -r - http
```

[![asciicast](https://asciinema.org/a/UXuc66hM4kCTZVK5Tc9gsxuoZ.svg)](https://asciinema.org/a/UXuc66hM4kCTZVK5Tc9gsxuoZ)
