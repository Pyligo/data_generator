influxdb to csv： https://github.com/mukatee/influxdb-dumper
csv to influxdb：https://github.com/fabio-miranda/csv-to-influxdb

先获得csv文件，之后默认csv中所有列均作为fields，并写一个eqpt_no值作为tag一起写入数据库中

## Usage

```
usage: csv-to-influxdb.py [-h] -i [INPUT] [-d [DELIMITER]] [-s [SERVER]]
                          [--ssl] [-u [USER]] [-p [PASSWORD]] --dbname
                          [DBNAME] [--create] [-m [METRICNAME]]
                          [-tc [TIMECOLUMN]] [-tf [TIMEFORMAT]] [-tz TIMEZONE]
                          [--fieldcolumns [FIELDCOLUMNS]]
                          [--tagcolumns [TAGCOLUMNS]] [-g] [-b BATCHSIZE]

Csv to influxdb.

optional arguments:
  -h, --help            show this help message and exit
  -i [INPUT], --input [INPUT]
                        Input csv file.
  -d [DELIMITER], --delimiter [DELIMITER]
                        Csv delimiter. Default: ','.
  -s [SERVER], --server [SERVER]
                        Server address. Default: localhost:8086
  --ssl                 Use HTTPS instead of HTTP.
  -u [USER], --user [USER]
                        User name.
  -p [PASSWORD], --password [PASSWORD]
                        Password.
  --dbname [DBNAME]     Database name.
  --create              Drop database and create a new one.
  -m [METRICNAME], --metricname [METRICNAME]
                        Metric column name. Default: value
  -tc [TIMECOLUMN], --timecolumn [TIMECOLUMN]
                        Timestamp column name. Default: timestamp.
  -tf [TIMEFORMAT], --timeformat [TIMEFORMAT]
                        Timestamp format. Default: '%Y-%m-%d %H:%M:%S' e.g.:
                        1970-01-01 00:00:00
  -tz TIMEZONE, --timezone TIMEZONE
                        Timezone of supplied data. Default: UTC
  --fieldcolumns [FIELDCOLUMNS]
                        List of csv columns to use as fields, separated by
                        comma, e.g.: value1,value2. Default: value
  --tagcolumns [TAGCOLUMNS]
                        List of csv columns to use as tags, separated by
                        comma, e.g.: host,data_center. Default: host
  -g, --gzip            Compress before sending to influxdb.
  -b BATCHSIZE, --batchsize BATCHSIZE
                        Batch size. Default: 5000.
```

## Example

使用docker compose启动，用之前已编译好的doctopus镜像（内含python3.6及各类包，较全），将csv及py文件挂载进去，跑这个py文件。

docker-compose.yaml中command命令：

`python  csv-to-influxdb.py --dbname GILAB --input report_csv/weiss-1.csv -m weiss-1 --tag weiss-1 --timestart 2020-05-15T00:00:00Z`

--dbname  ：数据库名字

--input ：读取的csv文件

-m :   数据库写入的表名

--tag：eqpt_no 设备名的值作为数据的tag

--timestart：从哪个时间开始生成数据



启动docker-compose文件：

`docker-compose -f docker-compose_weiss.yaml up -d`

查看程序日志

`docker logs -f --tail=20 gilab_weiss_data_generator`

