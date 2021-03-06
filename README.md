# CSV to OpenLDAP
[![MIT License](https://img.shields.io/badge/license-MIT-007EC7.svg?style=flat)](/LICENSE) [![Python](https://img.shields.io/badge/python-3.6-blue.svg)]()

A *simple* CSV to OpenLDAP system. It reads a .csv file, creates the users in OpenLDAP with a randomic password and sends an email to the user with the account info.

# Getting Started

## Installing

To install **CSV to OpenLDAP** you will need to:
```
git clone https://github.com/alefeans/csv-to-ldap.git && cd csv-to-ldap/
pip install -r requirements.txt
```

## Usage

From it's `--help` option:
```
python csv-to-ldap/main.py -h
usage: main.py [-h] [-a ADDRESS]
                    -u USER -w PASSWORD
                    -s SMTP_HOST
                    -p PORT
                    -e EMAIL
                    -r EMAIL_PASSWORD
                    -f FILE

CSV to OpenLDAP

optional arguments:
  -a ADDRESS, --address ADDRESS
                        OpenLDAP address, default is localhost

required arguments:
  -u USER, --user USER  OpenLDAP user (dn)
  -w PASSWORD, --password PASSWORD
                        OpenLDAP password
  -s SMTP_HOST, --smtp_host SMTP_HOST
                        SMTP host, e.g 'smtp-mail.outlook.com'
  -p PORT, --port PORT  SMTP port, e.g '587'
  -e EMAIL, --email EMAIL
                        email sender, e.g 'test@hotmail.com'
  -r EMAIL_PASSWORD, --email_password EMAIL_PASSWORD
                        email password
  -f FILE, --file FILE  CSV file to parse
```

## Example

Example with the [users.csv](/examples/users.csv) file using only the **required** parameters:

```
python csv-to-ldap/main.py -u 'cn=admin,dc=example,dc=org' -w admin -s 'smtp-mail.outlook.com' -p 587 -e 'user_test_py@hotmail.com' -r '<password>' -f examples/users.csv

INFO - SMTP - Authentication Successful
INFO - LDAP - Creating user 'finn' on OpenLDAP
INFO - SMTP - Sending account info to finn_mertens@ooo.com
INFO - LDAP - Creating user 'jake' on OpenLDAP
INFO - SMTP - Sending account info to jake_the_dog@ooo.com
INFO - LDAP - Creating user 'simon' on OpenLDAP
INFO - SMTP - Sending account info to ice_king@ooo.com
INFO - Finished. Total of 3 user(s) created
```

If the users already exists in OpenLDAP:

```
INFO - SMTP - Authentication Successful
WARN - LDAP -  entryAlreadyExists
WARN - LDAP -  entryAlreadyExists
WARN - LDAP -  entryAlreadyExists
INFO - Finished. Total of 0 user(s) created
```

The user entry in OpenLDAP:
```
DN: cn=finn,dc=example,dc=org - STATUS: Read - READ TIME: 2018-12-31T18:37:18.156810
    cn: finn mertens
        finn
    givenName: finn
    mail: finn_mertens@gmail.com
    objectClass: top
                 person
                 organizationalPerson
                 inetOrgPerson
    sn: mertens
    uid: finn
    userPassword: b'CzlkndNzGryd'
```

The email will be sended using this [message.txt](/examples/message.txt) template (feel free to edit, preserving the template substitutions):

![](/imgs/email_example.png)


## To Do

* Include *'must change password at first login'* policy.
* Include unit tests.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
