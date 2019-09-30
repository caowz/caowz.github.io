
<!--more-->

> 最近想下载一些地震数据,但是发现[IRIS](https://www.iris.edu/hq/)网站中的
[Wilber3](https://ds.iris.edu/wilber3/find_event)网页一直无法打开,因此尝试
使用脚本获取数据的方法,把对应的用法在这里记录.

## Web Service Fetch scripts
[Web Service Fetch scripts](https://seiscode.iris.washington.edu/projects/ws-fetch-scripts)
提供了4个脚本来获取地震数据以及地震相关的一些信息.

**1. FetchData (Perl)**

- 获取地震的时间序列波形以及台站的具体信息, **SAC** 文件仪器响应等相关信息的脚本

**2. FetchMetadata (Perl)**

- 获取台站的 metadata, 主要包含了台网,坐标通道等信息

**3. FetchEvent (Perl)**

- 获取地震的信息

**4. FetchSyn (Python)**

- 从[IRIS Syngine](http://service.iris.edu/irisws/syngine/1/)获取合成波形

上述提到的4个脚本可以在[Web Service Fetch scripts](https://seiscode.iris.washington.edu/projects/ws-fetch-scripts)
的网站上进行下载.

## FetchData
**FetchData** 是使用 **perl** 来编写的脚本,其具体的用法如下:
``` 
FetchData-2018.337: collect time series and related metadata (version 2018.337)
http://service.iris.edu/clients/

Usage: FetchData-2018.337 [options]

 Options:
  -v                Increase verbosity, may be specified multiple times
  -h                Print this help message, if multiple print more help
  -q                Be quiet, do not print anything but errors
  -N,--net          Network code, list and wildcards (* and ?) accepted
  -S,--sta          Station code, list and wildcards (* and ?) accepted
  -L,--loc          Location ID, list and wildcards (* and ?) accepted
  -C,--chan         Channel codes, list and wildcards (* and ?) accepted
  -Q,--qual         Quality indicator, by default no quality is specified
  -s starttime      Specify start time (YYYY-MM-DD,HH:MM:SS.ssssss)
  -e endtime        Specify end time (YYYY-MM-DD,HH:MM:SS.ssssss or #[SMHD])
  --lat min:max     Specify a minimum and/or maximum latitude range
  --lon min:max     Specify a minimum and/or maximum longitude range
  --radius lat:lon:maxradius[:minradius]
                      Specify circular region with optional minimum radius
  -l listfile       Read list of selections from file
  -b bfastfile      Read list of selections from BREQ_FAST file
  -a user:pass      User and password for access to restricted data

  -F [DC1[,DC2]]    Federate the request to multiple data centers if needed
                      Federation may be limited to an optional list of DCs
                      Output files are prefixed by data center identifiers

  -o outfile        Fetch time series data and write to output file
  -sd sacpzdir      Fetch SAC P&Zs and write files to sacpzdir
  -rd respdir       Fetch RESP and write files to respdir
  -m metafile       Write basic metadata to specified file
  -X SXMLfile       Write response-level StationXML to specified file
```
如果想查看上述的信息,在命令行输入下面的命令来查看:
```
./FetchData -h
```
**FetchData** 示例:
```
 ./FetchData-2018.337 -N _GSN -C BH* -s 2019-09-25,23:45:00 -e2019-09-26,00:30:00 -o test.mseed -rd resp -m test.metadata
```
从 **FetchData** 的使用说明中可以看到,其下载的文件格式是`mseed`的格式,可以
使用 `mseed2sac` 来提取所需要的 **SAC** 格式的文件.关于该脚本的具体
使用在其 **Usage** 中已经介绍的比较清楚了,这里就不再详细介绍了.

{{% admonition tip "Tip" %}}
在下载数据是最好下载对应的仪器响应文件,这样会方便后续的处理.
{{% /admonition %}}


## FetchMetadata
**FetchMetadata** 是使用 **perl** 来编写的脚本,其具体的用法如下:
```
FetchMetadata-2014.316: collect channel metadata (2014.316)
http://service.iris.edu/clients/

Usage: FetchMetadata-2014.316 [options]

 Options:
 -v                Increase verbosity, may be specified multiple times
 -N,--net          Network code, list and wildcards (* and ?) accepted
 -S,--sta          Station code, list and wildcards (* and ?) accepted
 -L,--loc          Location ID, list and wildcards (* and ?) accepted
 -C,--chan         Channel codes, list and wildcards (* and ?) accepted
 -s starttime      Specify start time (YYYY-MM-DD,HH:MM:SS)
 -e endtime        Specify end time (YYYY-MM-DD,HH:MM:SS)
 -ua date          Limit to metadata updated after date (YYYY-MM-DD,HH:MM:SS)
 -mts              Limit to metadata where criteria match time series data
 -X xmlfile        Write raw returned FDSN StationXML to xmlfile
 -l listfile       Read list of selections from file
 -b bfastfile      Read list of selections from BREQ_FAST file
 -sta              Print station level information, default is channel
 -resp             Request response level information, no details printed
 -A appname        Application/version string for identification

 -o outfile        Write basic metadata to specified file instead of printing
```

如果想查看上述的信息,在命令行输入下面的命令来查看:
```
./FetchMetadata -h
```
**FetchMetadata** 示例:
```
./FetchMetadata-2014.316 -N _GSN -s 2019-09-25,23:45:00 -e2019-09-26,00:30:00
```

**FetchMetadata** 输出的格式如下:
```
#net|sta|loc|chan|lat|lon|elev|depth|azimuth|dip|instrument|scale|scalefreq|scaleunits|samplerate|start|end
AU|MCQ||AX0|-54.4986|158.9561|14|0|0|0||3.2875E0|0|A|1|2008-04-28T00:00:00|2599-12-31T23:59:59
```

## FetchEvent
**FetchEvent** 是使用 **perl** 来编写的脚本,其具体的用法如下:
```
FetchEvent-2014.340: collect event information (2014.340)
http://service.iris.edu/clients/

Usage: FetchEvent-2014.340 [options]

 Options:
 -v                More verbosity, may be specified multiple times (-vv, -vvv)

 -s starttime      Limit to origins after time (YYYY-MM-DD,HH:MM:SS.sss)
 -e endtime        Limit to origins before time (YYYY-MM-DD,HH:MM:SS.sss)
 --lat min:max     Specify a minimum and/or maximum latitude range
 --lon min:max     Specify a minimum and/or maximum longitude range
 --radius lat:lon:maxradius[:minradius]
                     Specify circular region with optional minimum radius
 --depth min:max   Specify a minimum and/or maximum depth in kilometers
 --mag min:max     Specify a minimum and/or maximum magnitude
 --magtype type    Specify a magnitude type for magnitude range limits
 --cat name        Limit to origins from specific catalog (e.g. ISC, 'NEIC PDE', GCMT)
 --con name        Limit to origins from specific contributor (e.g. ISC, NEIC)
 --ua date         Limit to origins updated after date (YYYY-MM-DD,HH:MM:SS)

 --allorigins      Return all origins, default is only primary origin per event
 --allmags         Return all magnitudes, default is only primary magnitude per event
 --orderbymag      Order results by magnitude instead of time

 --evid id         Select a specific event by DMC event ID
 --orid id         Select a specific event by DMC origin ID

 -X xmlfile        Write raw returned XML to xmlfile
 -A appname        Application/version string for identification

 -o outfile        Write event information to specified file, default: console
```
如果想查看上述的信息,在命令行输入下面的命令来查看:
```
./FetchEvent -h
```
**FetchEvent** 示例:
```
./FetchEvent-2014.340  -s 2019-09-25,23:45:00 -e 2019-09-26,00:30:00
```
{{% admonition tip "Tip" %}}
在下载地震数据之前可以通过该命令得到的在某些特定的条件下的所有地震事件的信息.
这样可以根据地震的发震时刻来选择获取哪一段时间内的波形数据.
{{% /admonition %}}

## FetchSyn
**FetchSyn** 是使用 **Python** 来编写的脚本,其具体的用法如下:
```
usage: FetchSyn-2016.007 [-h] [-A AGENT] [-recfile RECEIVERFILE] [-u]
                         [-outfile OUTFILE] [-model MODEL]
                         [-format FORMATSTRING] [-label LABEL] [-C COMPONENTS]
                         [-units UNITS] [-scale SCALE] [-dt DT]
                         [-kernelwidth KERNELWIDTH] [-origin ORIGINSTRING]
                         [-s STARTSTRING] [-e ENDSTRING] [-N NETWORK]
                         [-S STATION] [-rlat RLAT] [-rlon RLON]
                         [-Ncode NETCODE] [-Scode STACODE] [-Lcode LOCCODE]
                         [-evid EVID] [-slat SLAT] [-slon SLON] [-sdepm SDEPM]
                         [-mt MOMENTSTRING] [-dc DOUBLECOUPLESTRING]
                         [-force FORCESTRING]

optional arguments:
  -h, --help            show this help message and exit
  -A AGENT, --A AGENT   UserAgent info
  -recfile RECEIVERFILE, --recfile RECEIVERFILE, --ReceiverFile RECEIVERFILE
                        receiver file name with NET STATION
  -u, --u               extended HELP MENU
  -outfile OUTFILE, --outfile OUTFILE
                        output file label
  -model MODEL, --model MODEL
                        model from list:
                        http://ds.iris.edu/ds/products/syngine
  -format FORMATSTRING, --format FORMATSTRING
                        miniseed or saczip
  -label LABEL, --label LABEL
                        label
  -C COMPONENTS, --C COMPONENTS, --components COMPONENTS
                        components: any combination of ZNERT
  -units UNITS, --units UNITS
                        displacement, velocity, or acceleration (or d,v,a)
  -scale SCALE, --scale SCALE
                        amplitude scaling factor
  -dt DT, --dt DT       sampling interval in sec
  -kernelwidth KERNELWIDTH, --kernelwidth KERNELWIDTH
                        width of sinc resampling kernel
  -origin ORIGINSTRING, -origintime ORIGINSTRING, --origin ORIGINSTRING, --origintime ORIGINSTRING
                        end time YYYY-MM-DDTHH:MM:SS
  -s STARTSTRING, -start STARTSTRING, -starttime STARTSTRING, --s STARTSTRING, --start STARTSTRING, --starttime STARTSTRING
                        source time YYYY-MM-DDTHH:MM:SS or PHASE+-offset
  -e ENDSTRING, -end ENDSTRING, -endtime ENDSTRING, --e ENDSTRING, --end ENDSTRING, -endtime ENDSTRING
                        end time YYYY-MM-DDTHH:MM:SS or PHASE+-offset or
                        offset_from_start
  -N NETWORK, -network NETWORK, --N NETWORK, --network NETWORK
                        network
  -S STATION, -station STATION, --S STATION, --station STATION
                        station
  -rlat RLAT, --rlat RLAT
                        reciver lat
  -rlon RLON, --rlon RLON
                        receiver lon
  -Ncode NETCODE, -networkcode NETCODE, --Ncode NETCODE, --networkcode NETCODE
                        networkcode
  -Scode STACODE, -stationcode STACODE, --Scode STACODE, --stationcode STACODE
                        stationcode
  -Lcode LOCCODE, -locationcode LOCCODE, --Lcode LOCCODE, --locationcode LOCCODE
                        locationcode
  -evid EVID, -eventid EVID, --evid EVID, --eventid EVID
                        event id
  -slat SLAT, --slat SLAT
                        source lat
  -slon SLON, --slon SLON
                        source lon
  -sdepm SDEPM, --sdepm SDEPM
                        source depth in meters
  -mt MOMENTSTRING, --mt MOMENTSTRING, --sourcemomenttensor MOMENTSTRING
                        moment: mrr,mtt,mpp,mrt,mrp,mtp in N-m (1 J = 1 N-m =
                        1e7 dyne-cm). Comma delimited, no spaces or brackets.
  -dc DOUBLECOUPLESTRING, --dc DOUBLECOUPLESTRING, --sourcedoublecouple DOUBLECOUPLESTRING
                        strike,dip,rake[,ScalarMoment_in_NM]
  -force FORCESTRING, --force FORCESTRING, -forces FORCESTRING, --forces FORCESTRING, --sourceforce FORCESTRING
                        force source in NM: Fr,Ft,Fp
```
如果想查看上述的信息,在命令行输入下面的命令来查看:
```
./FetchSyn -h
```
**FetchSyn** 示例:
```
```

