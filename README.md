Enviro-Pi
=========

I forked this project from the original author to wokr a bit further on the data presentation. The original project can be fount at  https://github.com/odlevakp/enviro-pi

### Hardware requirements

* Raspberry Pi 3
* SenseHat

### Installation and Software requirements

Update your package database and make sure you have all required packages,
then clone this git repository. Installation of supervisor is optional, it
is used to keep the processes running in background.

```bash
sudo apt update
sudo apt install supervisor python3-flask sqlite3 git sense-hat
git clone https://github.com/odlevakp/enviro-pi.git ~/enviro-pi
```


### Auto start

Supervisor was installed to keep your two processes alive. First one, `writer.py`, reads environmental information from sensehat and writes them into a database.
Second process, `webserver.py`, starts a web server and displays collected data. The configuration needed to run them using supervisor, is placed in the
supervisor subdirectory.

```bash
cd ~/enviro-pi

sudo ln -s /home/pi/enviro-pi/supervisor/* /etc/supervisor/conf.d/
sudo systemctl restart supervisor.service

sudo supervisorctl
```

You should see both processes running and good to go. Open you browser on `PI_IP_ADDRESS:8080`.


### Screenshots
![Status MacOS screenshot](http://files.phisolutions.eu/status.png "Status MacOS screenshot")
![Charts MacOS screenshot](http://files.phisolutions.eu/charts.png "Charts MacOS screenshot")
![Statistics Android screenshot](http://files.phisolutions.eu/statistics.png "Statistics Android screenshot")


### Playing with database

The writer process is storing environmental readings in a local SQLite database. You can use the CLI client
to make additional queries on the recorded dataset.


```sql
$ sqlite3 sensehat.db

sqlite> .headers on
sqlite> .mode column

sqlite> SELECT * FROM sensehat;

sqlite> SELECT MAX(humidity),
datetime(epoch, 'unixepoch', 'localtime') as date
FROM sensehat;
```

Please note that time is stored in the `epoch` column as time since epoch. Column `temp_prs` stores temperature from pressure, not used in the web interface.



License and Authors
-------------------
Copyright 2016 Pavol Odlevak

Using <a href="http://www.chartjs.org/">Chart.js</a> and <a href="http://momentjs.com/">Moment.js</a> available under the MIT license.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
