puppet-xdebug
=============

Puppet module for installing XDEBUG PHP Extension

Installs XDEBUG Support.
Depends on (tested with)
 - https://github.com/puppet/puppet-php.git

Example usage:

    class { 'xdebug': }

To set up CGI/CLI specific INI files:

    xdebug::config { 'cgi': }
    xdebug::config { 'cli': }

Maintained by: PuPHPet

GitHub: git@github.com:puppet/puppet-xdebug.git

Original Author: Stefan Kögel

GitHub: git@github.com:stkoegel/puppet-xdebug.git
