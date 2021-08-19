# Certificate-Install-Script
---
*The following comes from [Thomas Leister](https://thomas-leister.de/en/how-to-import-ca-root-certificate/)*
---
Before continuing, please ensure libnss3-tools is installed. This allows you to use certutil to install certificates in all browsers. 

Debian/Ubuntu/etc. 
```bash
sudo apt install libnss3-tools
```

After this is installed, you can download helper.sh or copy the following bash script and save it as <filename>.sh.
```bash
#!/bin/bash

### Script installs root.cert.pem to certificate trust store of applications using NSS
### (e.g. Firefox, Thunderbird, Chromium)
### Mozilla uses cert8, Chromium and Chrome use cert9

###
### Requirement: apt install libnss3-tools
###


###
### CA file to install (CUSTOMIZE!)
###

certfile="root.cert.pem"
certname="My Root CA"


###
### For cert8 (legacy - DBM)
###

for certDB in $(find ~/ -name "cert8.db")
do
    certdir=$(dirname ${certDB});
    certutil -A -n "${certname}" -t "TCu,Cu,Tu" -i ${certfile} -d dbm:${certdir}
done


###
### For cert9 (SQL)
###

for certDB in $(find ~/ -name "cert9.db")
do
    certdir=$(dirname ${certDB});
    certutil -A -n "${certname}" -t "TCu,Cu,Tu" -i ${certfile} -d sql:${certdir}
done
```

Next, edit certfile and certname, certfile needs to match the certificate name and path. Certname can be anything identifiable. 
```bash 
### certfile="/path/to/certfile.der"
### certname="Certicate name as it will appear under certificates"
```

Lastly, chmod helper.sh to make it executable and run. 
```bash
chmod +x helper.sh
./helper.sh
```