## About
Simple Vagrant setup to create multiple hyper-V VM`s for testing and developing [Epiphany](https://github.com/epiphany-platform/epiphany) clusters locally.

## Requirements

- Windows machine with Hyper-V [enabled](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)
- Latest [Vagrant](https://www.vagrantup.com/downloads.html)

## Usage

### Configuration

There are 2 ways of configuration.

1: you can quickly create a number of similar VM`s with only the basic config:

```Ruby
cfg = {
  :vm_count => 4, # If set to 0 a set of VM`s will be created with the definitions in boxes below. Of > 0 then the general cfg (vm_cpus, vm_memory) is used.
  :vm_prefix => "prefix", # Prefix that will be used for creating the clusters.
  :vm_cpus => 2, # Number of CPU cores each VM should have when using the vm_count.
  :vm_memory => 2048, # Amount of memory each VM should have when using the vm_count.
  :vm_box => "generic/ubuntu1804",  #"generic/rhel7", "centos/7", # Image to use for the VM`s.
  :ssh_priv => "/path/to/ssh/private/key",  # Path of the private SSH key that you want to use to connect to the VMs.
  :ssh_public => "/path/to/ssh/public/key",  # Path of the public SSH key that you want to use to connect to the VMs.
  :network_switch => "switchname"  # The network switch you want to use to connect the VMs to. THis must be created in Hyper-V manager before hand and needs to be an external switch.
}
```

With the above configuration 4 VM`s will be created labeled ```prefix1``` ... ```prefix4``` with the specifications defined in ```vm_cpus``` and ```vm_memory```. The hostnames will be the same is the created VM labels.

2: You can set the ```vm_count``` to 0 and define a number of custom VM`s based on the configuration in the boxes array:

```ruby
boxes = [
  {
    :name => "Kubernetes Master", # Name the machine will have in Hyper-V manager.
    :hostname => "kmaster", # Hostname this VM will get on the network.
    :memory => 2048, # Ammount of memory this VM will have.
    :cpus => 2 # Number of CPU cores this VM will have.
  },
  {
    :name => "Kubernetes Node 1", # Name the machine will have in Hyper-V manager.
    :hostname => "knode1", # Hostname this VM will get on the network.
    :memory => 2048, # Ammount of memory this VM will have.
    :cpus => 2 # Number of CPU cores this VM will have.
  },
  {
    :name => "Kubernetes Node 2", # Name the machine will have in Hyper-V manager.
    :hostname => "knode2", # Hostname this VM will get on the network.
    :memory => 2048, # Ammount of memory this VM will have.
    :cpus => 2 # Number of CPU cores this VM will have.
  }
]
```

This will create 3 VM`s with the defined settings.

The following settings of the basic config will be used for both approaches described above:

```Ruby
cfg = {
  :vm_box => "generic/ubuntu1804",  #"generic/rhel7", "centos/7", # Image to use for the VM`s.
  :ssh_priv => "/path/to/ssh/private/key",  # Path of the private SSH key that you want to use to connect to the VMs.
  :ssh_public => "/path/to/ssh/public/key",  # Path of the public SSH key that you want to use to connect to the VMs.
  :network_switch => "switchname"  # The network switch you want to use to connect the VMs to. THis must be created in Hyper-V manager before hand and needs to be an external switch.
}
```

### VM Management
You can use the normal [Vagrant commands](https://www.vagrantup.com/docs/cli) to manage the VM`s you create for the cluster. Few of the most common:

- ```vagrant up ...``` to create a new set of VM`s
- ```vagrant destroy ...``` to destroy a set of VM`s
- ```vagrant snapshot ...``` for creating and managing VM snapshots. Usefull when developing functionality that needs to be retested frequently.
