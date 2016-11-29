# Windows Provisioning with Chef

This will provision a Windows 2012 R2 vagrant with a test kitchen using chef

The following are required

  - ChefDK
  - Vagrant
  - VirtualBox
  - Vagrant WinRM plugin


  Clone the repo to a local folder e.g. c:\git

  ```sh
$ git clone https://github.com/pavkhural/chef-repo.git
$ cd c:\git\chef-repo\windows_provisioning
```

Start by resolving any berkshelf cookbook dependencies which will also create the berksfile.lock by running 'berks install'
  ```sh
C:\Git\chef-repo\windows_provisioning\berks install
Resolving cookbook dependencies...
Fetching 'windows_provisioning' from source at .
Fetching cookbook index from https://supermarket.chef.io...
Using windows_provisioning (0.1.0) from source at .
```

You will then need to create the test kitchen

```sh
C:\Git\chef-repo\windows_provisioning\kitchen create
-----> Starting Kitchen (v1.13.2)
-----> Creating <default-Windows2012R2>...
       Bringing machine 'default' up with 'virtualbox' provider...
       ==> default: Importing base box 'mwrock/Windows2012R2'...
```
Once the kitchen has been created you should be able to see it as created in the list
```sh
C:\Git\chef-repo\windows_provisioning\kitchen list
Instance               Driver   Provisioner  Verifier  Transport  Last Action
default-Windows2012R2  Vagrant  ChefZero     Inspec    Winrm      Created
```

You can then converge the kitchen buy running 'kitchen converge' this will install the basic dependencies required for chef-zero to communicate with the VM and install the client and basic cookbook dependencies.
```sh
C:\Git\chef-repo\windows_provisioning [master ≡ +0 ~1 -0 !]> kitchen converge
-----> Starting Kitchen (v1.13.2)
-----> Converging <default-Windows2012R2>...
       Preparing files for transfer
       Preparing dna.json
       Resolving cookbook dependencies with Berkshelf 5.1.0...
       Removing non-cookbook files before transfer
       Preparing validation.pem
       Preparing client.rb
-----> Installing Chef Omnibus (install only if missing)
       Downloading package from https://packages.chef.io/files/stable/chef/12.16.42/windows/2012r2/chef-c
1-x64.msi
       Download complete.
       Successfully verified C:\Users\vagrant\AppData\Local\Temp\chef-client-12.16.42-1-x64.msi
       Installing Chef Omnibus package C:\Users\vagrant\AppData\Local\Temp\chef-client-12.16.42-1-x64.msi
       Installation complete
       Transferring files to <default-Windows2012R2>
       Starting Chef Client, version 12.16.42
       Creating a new client identity for default-Windows2012R2 using the validator key.
       resolving cookbooks for run list: ["windows_provisioning::default"]
       Synchronizing Cookbooks:
         - windows_provisioning (0.1.0)
       Installing Cookbook Gems:
       Compiling Cookbooks...
       Converging 0 resources

       Running handlers:
       Running handlers complete
       Chef Client finished, 0/0 resources updated in 28 seconds
       Finished converging <default-Windows2012R2> (1m52.76s).
-----> Kitchen is finished. (1m58.21s)
```

The defalt.rb recipe can then be modified to test any particular recipes or configurations and the kitchen status should also show as 'converged'

```sh
C:\Git\chef-repo\windows_provisioning [master ≡ +0 ~1 -0 !]> kitchen status
Instance               Driver   Provisioner  Verifier  Transport  Last Action
default-Windows2012R2  Vagrant  ChefZero     Inspec    Winrm      Converged
```
