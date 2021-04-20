# webpi Cookbook

[![Cookbook Version](https://img.shields.io/cookbook/v/webpi.svg)](https://supermarket.chef.io/cookbooks/webpi)
[![CI State](https://github.com/sous-chefs/webpi/workflows/ci/badge.svg)](https://github.com/sous-chefs/webpi/actions?query=workflow%3Aci)
[![OpenCollective](https://opencollective.com/sous-chefs/backers/badge.svg)](#backers)
[![OpenCollective](https://opencollective.com/sous-chefs/sponsors/badge.svg)](#sponsors)
[![License](https://img.shields.io/badge/License-Apache%202.0-green.svg)](https://opensource.org/licenses/Apache-2.0)

Microsoft Web Platform Installer (WebPI) automates the installation of Microsoft's entire Web Platform. This cookbook makes it easy to get WebpiCmdLine.exe the lightweight CLI version of WebPI onto a Windows node. It also exposes a resource for installing WebPI products idempotently.

## Maintainers

This cookbook is maintained by the Sous Chefs. The Sous Chefs are a community of Chef cookbook maintainers working together to maintain important cookbooks. If you’d like to know more please visit sous-chefs.org or come chat with us on the Chef Community Slack in #sous-chefs.

## Requirements

### Platforms

- Windows 7
- Windows Server 2008 R2
- Windows 8, 8.1
- Windows Server 2012 (R1, R2)

### Chef

- Chef 12.6+

### Cookbooks

- windows 2.0+

## Attributes

- `node['webpi']['home']` - location to install WebPI files to. Default is `%SYSTEMDRIVE%\webpi`
- `node['webpi']['url']` - Path to download webpi zip.
- `node['webpi']['xmlpath']` - Optional path to custom xml file.
- `node['webpi']['install_method']` - Method of installing webpi via msi or zip. Default is msi.
- `node['webpi']['msi']` - Location of msi file to use.
- `node['webpi']['msi_package_name']` - Windows package name, found in registry, for msi. Defaults to "Microsoft Web Platform Installer 4.5"

## Resources

### webpi_product

#### Actions

- :install: install a product using WebpiCmdLine

#### Properties

- product_id: name property. Specifies the ID of a product to install.
- accept_eula: specifies that WebpiCmdline should auto-accept EULAs. Default is false.
- returns: specifies the return value(s) expected for a successful installation. Can be a single integer or array of integers. Default is [0, 42]

#### Examples

Install IIS 7 Recommended Configuration (will install IIS 8 on Windows 2012 despite the name)

```ruby
webpi_product 'IIS7' do
  accept_eula true
  action :install
end
```

Install Windows PowerShell 2.0

```ruby
webpi_product 'PowerShell2' do
  accept_eula true
  action :install
end
```

Install Windows Azure Powershell 1.0 (will return a 3010 exit code to signify a successful installation that requires a reboot)

```ruby
webpi_product 'WindowsAzurePowerShellGet' do
  accept_eula true
  action :install
  returns 3010
end
```

### webpi_application

#### Actions

- :install: install an application using WebpiCmdLine

#### Properties

- app_id: name property. Specifies the ID of an application to install.
- accept_eula: specifies that WebpiCmdline should auto-accept EULAs. Default is false.
- suppress_reboot: specifies if we need to stop or suppress the reboot. Default is True.
- iis_express: used only with applications that intergrate with IIS. Default value is False.
- mysql_password: Used to give a mysql password for application which requires it.
- sql_password: Used to give a sql password for application which requires it.
- other_options: Any other options related to applications needed to be appended. Default value is empty string.
- returns: specifies the return value(s) expected for a successful installation. Can be a single integer or array of integers. Default is [0, 42]

#### Examples

Installing WebMatrix while Accepting the EULA and also install app to IISExpress
Ex: >WebPICMD.exe /Install /Products:WebMatrix /AcceptEula /IISExpress

```ruby
webpi_application 'WebMatrix' do
  accept_eula true
  iis_express true
  action  :install
end
```

Installing an application to MicrosoftAzure ServiceFabric CoreSDK
Ex: >WebPICMD.exe /Install /Applications:MicrosoftAzure-ServiceFabric-CoreSDK /AcceptEula

```ruby
webpi_application 'MicrosoftAzure-ServiceFabric-CoreSDK' do
  accept_eula true
  action :install
end
```

```ruby
password = data_bag_item('secrets','mysqlpassword')['password'] # Should be a encrypted data bag
webpi_application 'AcquiaDrupal' do
  accept_eula    true
  mysql_password password # To be set from encrypted databag
  action         :install
end
```

## Usage

### default

Downloads and unzips `WebpiCmdLine.exe` to the location specified by `node['webpi']['home']`. `WebpiCmdLine.exe` is used required by the `webpi_product` LWRP for taking all actions.

## Contributors

This project exists thanks to all the people who [contribute.](https://opencollective.com/sous-chefs/contributors.svg?width=890&button=false)

### Backers

Thank you to all our backers!

![https://opencollective.com/sous-chefs#backers](https://opencollective.com/sous-chefs/backers.svg?width=600&avatarHeight=40)

### Sponsors

Support this project by becoming a sponsor. Your logo will show up here with a link to your website.

![https://opencollective.com/sous-chefs/sponsor/0/website](https://opencollective.com/sous-chefs/sponsor/0/avatar.svg?avatarHeight=100)
![https://opencollective.com/sous-chefs/sponsor/1/website](https://opencollective.com/sous-chefs/sponsor/1/avatar.svg?avatarHeight=100)
![https://opencollective.com/sous-chefs/sponsor/2/website](https://opencollective.com/sous-chefs/sponsor/2/avatar.svg?avatarHeight=100)
![https://opencollective.com/sous-chefs/sponsor/3/website](https://opencollective.com/sous-chefs/sponsor/3/avatar.svg?avatarHeight=100)
![https://opencollective.com/sous-chefs/sponsor/4/website](https://opencollective.com/sous-chefs/sponsor/4/avatar.svg?avatarHeight=100)
![https://opencollective.com/sous-chefs/sponsor/5/website](https://opencollective.com/sous-chefs/sponsor/5/avatar.svg?avatarHeight=100)
![https://opencollective.com/sous-chefs/sponsor/6/website](https://opencollective.com/sous-chefs/sponsor/6/avatar.svg?avatarHeight=100)
![https://opencollective.com/sous-chefs/sponsor/7/website](https://opencollective.com/sous-chefs/sponsor/7/avatar.svg?avatarHeight=100)
![https://opencollective.com/sous-chefs/sponsor/8/website](https://opencollective.com/sous-chefs/sponsor/8/avatar.svg?avatarHeight=100)
![https://opencollective.com/sous-chefs/sponsor/9/website](https://opencollective.com/sous-chefs/sponsor/9/avatar.svg?avatarHeight=100)
