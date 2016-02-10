# write_onos
The write_onos is a collectd plugin that writes obtained values to ONOS collector service. This is written in python as python module.

# Requirements
  Collectd 5.4 or later (for the Python plugin)
  Python 2.7 or later (Note: pyCurl also is required to interface libcurl)
  A running ONOS collector service
  
# Version Information
  collectd write_onos does not currently maintain release versions. The policy is that the master branch should always be stable and production ready.
  
# Configuration

  The plugin requires some configuration. This is done by passing parameters via the config section in your Collectd config. The following parameters are recognized:
  
    host: This is the hostname or IP address of ONOS controller where collector service is listening for metrics. (e.g. "192.168.1.1")
  
    port: This is collector's listening port number (e.g. "8181")
  
    user: This is required user name for authentication (e.g."karaf")
  
    password: This is required password (e.g. "karaf")
  
    disks: You can define the list of disks/partitions for metrics in this parameter.  (e.g. ["sda1", "sda2", "sda5"])
  
    networks: You can define the list of interfaces for metrics here.  (e.g. ["eth0", "eth1"])
  
# Example

  The following is an example Collectd configuration for this plugin:

    Interval 60
    LoadPlugin interface
    LoadPlugin load
    LoadPlugin cpu
    LoadPlugin memory
    LoadPlugin disk
    LoadPlugin python

    <LoadPlugin python>
        Globals true
    </LoadPlugin>

    <Plugin python>
        ModulePath "/etc/collectd/modules/"
        LogTraces true
        Interactive false
        Import "write_onos"
      <Module write_onos>
         host "192.168.1.5"
         port "8181"
         user "karaf"
         password "karaf"
         disks ["sda1", "sda2", "sda5"]
         networks ["eth0", "eth1"]
      </Module>
    </Plugin>

    <Plugin cpu>
        ReportByState true
        ReportByCpu false
        ValuesPercentage false
    </Plugin>


# Operational Notes

  The plugin needs to parse Collectd cpu, load, memory, disk and network plugin data. It is therefore, we need to enable these plugins with the config file of collectd alogn with write_onos. Please give correct moudule path in collectd config file, in above example the "write_onos.py" file is located in /etc/collectd/module folder.


