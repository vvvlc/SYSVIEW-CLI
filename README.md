# SYSVIEW-CLI

`sysview-cli` is a uss wrapper around Sysview REXX api that allows to invoke Sysview commands from uss commandline.

For example:
```
~ # sysview-cli "ASPERF; SELECT JOBNAME = ZE*" | less -S
R4
MPROF024I*Switched from profile VLCVI01 to DEFAULT
T|SYSVIEW|15.0|CA31|ASPERF|05/27/19|07:46:13|
I Formats DEFAULT STORAGE
I Status  NoSRT NoLIM   SEL NoDST NoPFX NoOWN NoUPD NoPRT NoCAP
I           CP   IFA   IIP
I Factor   256   426   426
H|Cmd     |ASID|Jobname |JobStat|Type| Jobnr|Stepname|Procstep|JobClass|ClockTime| CPUTime|  CPTime|    CP%| IIPTime|   IIP%| IFATime|   IFA%|EnclTime|  ePct%|   RealStg|   FreeStg|    AuxStg|    WorkSet|    SCMStg|  GRealStg|   GAuxStg    |   IOCount|ASCBEJST|ASCBSRBT|ASCBEATT|TaskOnCP| SRBonCP|ASSTonCP|ASSBPHTM|zIIPPHTM| IFAPHTM|TimezIIP       |zIIPonCP|zIIPENCT|SUPCPENC|ASSBASST|ASSBENCT| TimeIFA| IFAonCP| IFAENCT|IFACPENC|PHTMBase|EnctBase|EIIPBase|EIFABase|ASSBIOCT|ASSBIIPT|zAAPzIIP|     CSA|    E-CSA|     SQA|    E-SQA|     G-CSA|       G-PVT|ASSBJBNI |ASSBJBNS|ASCB    |ASSB    |Description
D|        |0214|ZEMAN02 |LSW    |TSU | 27050|CATSO   |        |TSU     | 05:15:37|00:03:22|00:03:21| 99.61%|0.788527|  0.39%|        |       |0.882201|  0.44%|  36388864|          |          |   36388864|  14671872|  18321408|              |   3007514|00:03:18|2.821540|0.046862|00:03:18|2.821540|0.000528|0.882201|0.788527|        |               |        |0.788527|0.000188|0.000528|0.093673|        |        |        |        |        |0.092270|0.778720|        |        |0.722679|        |     136|     2456|      96|    10208|   1048576|  1093664768|         |ZEMAN02 |00F0ED00|06A48000|
```
## Usage
```
sysview-cli <SYSVIEW COMMAND>
sysview-cli JOBSUM
```

To chain commands use `;`
```
sysview-cli "PRE VLCVI*;JOBSUM; SORT JOBNAME DESC"
```

When output is too wide use `less -S` to enable scrolling in terminal window
```
sysview-cli "ASPERF; SELECT JOBNAME = ZOWE*" | less -S
```

To drive `sysview-cli` from your workstation use `ssh`
```
ssh your_mf_host '/path/sysview-cli "asperf ; SELECT JOBNAME = M*"'
```

### CNM4BLOD is not on LINKLIST
To override default Sysview loadlib define `STEPLIB` environment variable
```
STEPLIB=MY.CUSTOM.CNM4BLOD sysview-cli JOBSUM'
```

### Integration with other *nix tools
This command skips first 8 lines, extracts 4th column
``` 
sysview-cli "ASPERF; SELECT JOBNAME = ZOWE*" | tail +8 | awk -F '|' '{print $4}'
```
**NOTE:** field separator is `|`

## Installation
 1) upload sysview-cli to a uss folder
 2) chmod +x sysview-cli
