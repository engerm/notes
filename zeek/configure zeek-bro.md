# Configure zeek/bro

The following are basic steps for configuring a standalone zeek configuration. The changes and modifications below are __NOT__ meant for a production server without further review and configuration.

_The below changes and modification were used to configure zeek on Kali and CentOS._


## configure zeeks's local.bro

1. copy existing local.bro file. Execute:

    ```
    cp /usr/local/bro/share/bro/site/local.bro /usr/local/bro/share/bro/site/local.bro.`date +%Y%m%d`.orig
    ```

1. enable various protocol analyzers; some protocol analyzers are process intensive and generate lots of events. To enable the protocol analyzers, execute:

    ```
    sed -i -e "s/# @load policy\/protocols\/conn\/vlan-logging/@load policy\/protocols\/conn\/vlan-logging # enabling/g" /usr/local/bro/share/bro/site/local.bro
    sed -i -e "s/# @load policy\/protocols\/conn\/mac-logging/@load policy\/protocols\/conn\/mac-logging # enabling/g" /usr/local/bro/share/bro/site/local.bro
    echo -e "\n\n# adding additional scripts" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/files/x509/log-ocsp\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/frameworks/dpd/detect-protocols\t# enabling - detect protocols on non-standard ports" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/frameworks/dpd/packet-segment-logging\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/frameworks/files/extract-all-files\t# enabling - extract files to disk" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/frameworks/files/hash-all-files\t# enabling - hash seen files" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/misc/capture-loss\t# enabling - monitor bro's caputure loss" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/misc/loaded-scripts\t# enabling - monitor bro's loaded scripts" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/misc/weird-stats\t# enabling - monitor bro's weird stats" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/conn/known-hosts\t# enabling - daily known hosts" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/conn/known-services\t# enabling - daily known services" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/conn/weirds\t# enabling - log wierd events" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/dhcp/software\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/ftp/detect-bruteforcing\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/http/header-names\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/http/var-extraction-cookies\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/http/var-extraction-uri\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/krb/ticket-logging\t# enabling - enable Kerberos hash logging" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/mysql/software\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/rdp/indicate_ssl\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/smb\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/smtp/detect-suspicious-orig\t# enabling - smtp geolocation" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/ssl/expiring-certs\t# enabling - track expired/expiring certs" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/ssl/validate-ocsp\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/ssl/weak-keys\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/modbus/known-masters-slaves.bro\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/modbus/track-memmap.bro\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/smtp/entities-excerpt.bro\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/smtp/blocklists.bro\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/protocols/ssl/expiring-certs.bro\t# enabling" >> /usr/local/bro/share/bro/site/local.bro

    ```

## configure zeeks's extract-all-files.bro

1. copy existing extract-all-files.bro file. Execute:

    ```
    cp /usr/local/bro/share/bro/policy/frameworks/files/extract-all-files.bro /usr/local/bro/share/bro/policy/frameworks/files/extract-all-files.bro.`date +%Y%m%d`.orig
    ```

    1. Create directory to temporarily extract the files:

    ``` 
    mkdir -p /var/data/zeek/temp-extracted/
    ```

## configure zeeks's hash-all-files.bro

1. copy existing extract-all-files.bro file. Execute:

    ```
    cp /usr/local/bro/share/bro/policy/frameworks/files/hash-all-files.bro /usr/local/bro/share/bro/policy/frameworks/files/hash-all-files.bro.`date +%Y%m%d`.orig
    ```

1. make changes to add analyzers. Execute:

    ```
    sed -i -e '$s/^/\n\t# custom File Analyzers:\n/' /usr/local/bro/share/bro/policy/frameworks/files/hash-all-files.bro
    sed -i -e '$s/^/\t# https:\/\/www.bro.org\/sphinx\/script-reference\/file-analyzers.html\n/' /usr/local/bro/share/bro/policy/frameworks/files/hash-all-files.bro
    sed -i -e '$s/^/\tFiles::add_analyzer(f, Files::ANALYZER_SHA256);\n/' /usr/local/bro/share/bro/policy/frameworks/files/hash-all-files.bro
    ```

## deploy changes to zeek

1. deploy the above changes to zeek

    ```
    broctl deploy
    ```
    
    * result:

    ```
    checking configurations ...
    installing ...
    removing old policies in /usr/local/bro/spool/installed-scripts-do-not-touch/site ...
    removing old policies in /usr/local/bro/spool/installed-scripts-do-not-touch/auto ...
    creating policy directories ...
    installing site policies ...
    generating standalone-layout.bro ...
    generating local-networks.bro ...
    generating broctl-config.bro ...
    generating broctl-config.sh ...
    stopping ...
    stopping bro ...
    starting ...
    starting bro ...
    ```
    

## OPTIONAL configure zeek's workers in node.cfg

Configure zeek's workers in node.cfg.

1. if a backup copy of node.cfg oes not exist, then copy existing node.cfg file. Execute:

    ```
    cp /usr/local/bro/etc/node.cfg /usr/local/bro/etc/node.cfg.`date +%Y%m%d`.orig
    ```

1. make necessary changes to node.cfg workers as needed.


## OPTIONAL configure zeek's intel

1. Add properly formatted intel files to `/usr/local/bro/share/bro/policy/frameworks/intel/`.

2. enable intel in local.bro. Execute:

    ```
    echo -e "@load policy/frameworks/intel/do_notice\t# enabling - intel alerts" >> /usr/local/bro/share/bro/site/local.bro
    echo -e "@load policy/frameworks/intel/seen/\t# enabling" >> /usr/local/bro/share/bro/site/local.bro
    ```

## OPTIONAL: install package manager

1. install package manager. Execute:

    ```
    pip install bro-pkg
    bro-pkg autoconfig
    ```

1. enable package manager. Execute:

    ```
    echo -e "@load packages\t# enabling - package manager" >> /usr/local/bro/share/bro/site/local.bro
    ```

1. search all available packages. Execute:

    ```
    bro-pkg search /.*/
    ```

### install packages from package manager

The following is an example to add fuzzy hash (ssdeep) via the package manager.

1. install package for detecting smb1. Execute:

    ```
    bro-pkg install find_smbv1
    ```
    
    * result:

    ```
    The following packages will be INSTALLED:
      bro/klehigh/find_smbv1 (1.0.0)

    Proceed? [Y/n]
    Installed "bro/klehigh/find_smbv1" (1.0.0)
    Loaded "bro/klehigh/find_smbv1"
    ```

    NOTE: `/usr/local/bro/share/bro/site/packages` contains installed packages
    
1. deploy changes. Execute:

    ```
    broctl deploy
    ```

* See error logs for trouble shooting. Install package for fuzzy hash . Execute:

    ```
    bro-pkg install bro-fuzzy-hashing
    ```
    
    * result:

    ```
    The following packages will be INSTALLED:
      bro/j-gras/bro-fuzzy-hashing (0.3.0)

    Proceed? [Y/n] 
    Running unit tests for "bro/j-gras/bro-fuzzy-hashing"
    error: failed to run tests for bro/j-gras/bro-fuzzy-hashing: package build_command failed, see log in /root/.bro-pkg/logs/bro-fuzzy-hashing-build.log
    Proceed to install anyway? [N/y]
    ```

    * view the errors to troubleshoot. Execute:

    ```
    view /root/.bro-pkg/logs/bro-fuzzy-hashing-build.log
    ```
    
    * result:

    ```
    ...
    ...
    ...
    -- Could NOT find SSDeep (missing:  SSDEEP_LIBRARY SSDEEP_INCLUDE_DIR)
    -- Could NOT find TLSH (missing:  TLSH_LIBRARY TLSH_INCLUDE_DIR)
    -- Configuring incomplete, errors occurred!
    See also "/root/.bro-pkg/testing/bro-fuzzy-hashing/clones/bro-fuzzy-hashing/build/CMakeFiles/CMakeOutput.log".
    ...
    ...
    ...
    ```    


## references

* [Bro Package Manager](https://bro-package-manager.readthedocs.io/en/stable/quickstart.html)
