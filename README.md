# vagrant-vmware
Ubuntu VM on Vagrant and VMware Workstation Pro

This project focus Ubuntu VM on Vagrant and VMware workstation Pro, where Vagrantfile with Virtualbox was used previously.
1. Download and install Broadcom VMware Workstation Pro, where version 17.5.2 build-23775571 with individual licence was used for my study. Please follow the official web site for latest release and availability.
2. By default, VirtualBox is the default provider for Vagrant. You will need to install plugin for VMWare provider. I followed this link for updating the VMware plugin,vagrant plugin install vagrant-vmware-desktop. The link is https://developer.hashicorp.com/vagrant/docs/providers/vmware/installation 

*** Read and follow the instructions...
Installation of the Vagrant VMware provider requires two steps. First the Vagrant VMware Utility must be installed. This can be done by downloading and installing the correct system package from the Vagrant VMware Utility downloads page. https://developer.hashicorp.com/vagrant/downloads/vmware

Next, install the Vagrant VMware provider plugin using the standard plugin installation procedure: vagrant plugin install vagrant-vmware-desktop
***

$ vagrant plugin
Usage: vagrant plugin <command> [<args>]

Available subcommands:
     expunge
     install
     license
     list
     repair
     uninstall
     update

For help on any individual command run `vagrant plugin COMMAND -h`
        --[no-]color                 Enable or disable color output
        --machine-readable           Enable machine readable output
    -v, --version                    Display Vagrant version
        --debug                      Enable debug output
        --timestamp                  Enable timestamps on log output
        --debug-timestamp            Enable debug output with timestamps
        --no-tty                     Enable non-interactive output

CICD-Student@DESKTOP-FNUA4LH MINGW64 ~/securityclass/vagrant-vmware
$ vagrant plugin list
vagrant-scp (0.5.9, global)
vagrant-vbguest (0.32.0, global)
  - Version Constraint: > 0
vagrant-vmware-desktop (3.0.3, global)


3. Same working Vagrantfile for Virtualbox could be reused with a slightly modification for config.vm.provider "vmware_desktop" as shown below.
4. Make sure Virutalbox instances are disabled or shutdown to aviod resource conflict as I do not have chance to do so.

*** console output for bring up Ubuntu VM
$ vagrant -v
Vagrant 2.4.1

$ cat Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2004"
  config.vm.provider "vmware_desktop" do |vb|
     vb.memory = "1024"
   end
end

$ vagrant up
Bringing machine 'default' up with 'vmware_desktop' provider...
==> default: Cloning VMware VM: 'generic/ubuntu2004'. This can take some time...
==> default: Checking if box 'generic/ubuntu2004' version '4.3.2' is up to date...
==> default: Verifying vmnet devices are healthy...
==> default: Preparing network adapters...
==> default: Starting the VMware VM...
==> default: Waiting for the VM to receive an address...
==> default: Forwarding ports...
    default: -- 22 => 2222
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Configuring network adapters within the VM...

CICD-Student@DESKTOP-FNUA4LH MINGW64 ~/securityclass/vagrant-vmware
$ vagrant global-status
id       name    provider       state    directory
------------------------------------------------------------------------------------------------
c460533  jenkins virtualbox     poweroff C:/Users/CICD-Student/cicdclass/Project-Jenkins-Docker
b01ad49  control virtualbox     poweroff C:/Users/CICD-Student/cicdclass/Project-VM-Ansible
6d6a998  web01   virtualbox     poweroff C:/Users/CICD-Student/cicdclass/Project-VM-Ansible
2f79f0c  default virtualbox     poweroff C:/Users/CICD-Student/cicdclass/ubuntu
a210109  default vmware_desktop running  C:/Users/CICD-Student/securityclass/vagrant-vmware

The above shows information about all known Vagrant environments
on this machine. This data is cached and may not be completely
up-to-date (use "vagrant global-status --prune" to prune invalid
entries). To interact with any of the machines, you can go to that
directory and run Vagrant, or you can use the ID directly with
Vagrant commands from any directory. For example:
"vagrant destroy 1a2b3c4d"

CICD-Student@DESKTOP-FNUA4LH MINGW64 ~/securityclass/vagrant-vmware
$
$ vagrant ssh
vagrant@ubuntu2004:~$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 00:0c:29:d6:24:a4 brd ff:ff:ff:ff:ff:ff
    inet 192.168.94.129/24 brd 192.168.94.255 scope global dynamic eth0
       valid_lft 1718sec preferred_lft 1718sec
    inet6 fe80::20c:29ff:fed6:24a4/64 scope link
       valid_lft forever preferred_lft forever
vagrant@ubuntu2004:~$
root@ubuntu2004:~# cat /etc/os-release
NAME="Ubuntu"
VERSION="20.04.6 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.6 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal
root@ubuntu2004:~#

