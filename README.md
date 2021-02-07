# Connections_to_IBMDB2_Server through Python in Linux server

# connect from application server to IBM DB2 server (Redhat linux server) through Python 

please follow the below steps connecting to IBM DB2 server from application servers

- Once logged into Linux server, install python 3.6 or above
- install PIP for downloading/installing libraries
- check for the connectivity (like network, firewall, port, ping) from python installed server( application/destination server) to Oracle DB server (target server)
- perform the below python coding and save the script as .py file (example: connect.py)
- run the script the get the output in python (python3# connect.py)
- Please follow the below steps installation Python

# Python Installation in Redhat Linux:
Start by making sure your system is up-to-date:
yum update

# Compilers and related tools:
yum groupinstall -y "development tools"

# Libraries needed during compilation to enable all features of Python:
yum install -y zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel expat-devel

# If you are on a clean "minimal" install of CentOS you also need the wget tool:
yum install -y wget

# Python 3.6.3:
tar xf Python-3.6.3.tar.xz cd Python-3.6.3 ./configure --prefix=/usr/local --enable-shared LDFLAGS="-Wl,-rpath /usr/local/lib" make && make altinstall

# Strip the Python 3.6 binary:
strip /usr/local/lib/libpython3.6m.so.1.0

# Then execute it using Python 2.7 and/or Python 3.6: (download the get-pip.py from https://bootstrap.pypa.io/get-pip.py)
python3.6 get-pip.py

# yum info python34 pythonu
Loaded plugins: product-id, search-disabled-repos, security, subscription-manager
Installed Packages
Name        : python34
Arch        : x86_64
Version     : 3.4.10
Release     : 4.el6
Size        : 31 k
Repo        : installed
From repo   : epel
Summary     : Version 3 of the Python programming language aka Python 3000
URL         : http://www.python.org/
License     : Python
Description : Python 3 is a new version of the language that is incompatible with the 2.x
            : line of releases. The language is mostly the same, but many details, especially
            : how built-in objects like dictionaries and strings work, have changed
            : considerably, and a lot of deprecated features have finally been removed.

Available Packages
Name        : python34
Arch        : i686
Version     : 3.4.10
Release     : 4.el6
Size        : 51 k
Repo        : epel
Summary     : Version 3 of the Python programming language aka Python 3000
URL         : http://www.python.org/
License     : Python
Description : Python 3 is a new version of the language that is incompatible with the 2.x
            : line of releases. The language is mostly the same, but many details, especially
            : how built-in objects like dictionaries and strings work, have changed
            : considerably, and a lot of deprecated features have finally been removed.

# yum install epel-release
Loaded plugins: product-id, search-disabled-repos, security, subscription-manager Setting up Install Process Package epel-release-6-8.noarch already installed and latest version Nothing to do

# yum install python-pip
Loaded plugins: product-id, search-disabled-repos, security, subscription-manager Setting up Install Process Package python-pip-7.1.0-2.el6.noarch already installed and latest version Nothing to do



# connect from Application server (RedHat Linux server) to IBM DB2 server (RedHat Linux server) through python.

# cat Connect_DB2.py

# connect from PI server to DB2 server.  create a Database table in DB2 linux server from another linux server using python.

import pandas as pd
import ibmdb2
from ibm_db import connect
import ibm_db_dbi

try:
    conn =ibm_db_dbi.connect('DATABASE=<DBName>;'
                     'HOSTNAME=<targetServerName>;'
                     'PORT=50000;'
                     'PROTOCOL=TCPIP;'
                     'UID=<DBInstanceID>;'
                     'PWD=<password>;', '', '')
except Exception as err:
    print("Exception error while creating the connection - ", err)

else:
    try:
        cur = conn.cursor()
        DB_create = ''' CREATE TABLE AIOPS_Output (
            Date VARCHAR(100),
            Anomaly VARCHAR(100),
            Algo VARCHAR(100),
            Min INTEGER,
            Max INTEGER,
            Server_Name VARCHAR(100),
            KPI_Name VARCHAR(100),
            KPI_Value VARCHAR(100),
            Modified_Date VARCHAR(100)
        )
        '''

        cur.execute (DB_create)
        print('Table Created')


        DB_Insert = """ INSERT INTO AIOPS_Output VALUES ( '1201204120000000', '98%', 'IQRAD', '10%', '98%','Server1','<tablename>', '20', '1201204120000000') """
        cur.execute(DB_Insert)



    except Exception as err:
        print("Error while inserting the data - ", err)

    else:
        print("Insert completed")
        conn.commit()


finally:
    cur.close()
    conn.close()



