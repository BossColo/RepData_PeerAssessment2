# Reproducible Research: Peer Assessment 2

<style>
 th,td{
   padding:2px 5px 2px 5px;
 }
</style>

## Introduction
For Assignment 2 of this course, we are looking at Storm Data from NOAA and The National Weather Service. We will be examining different storm events, 
specifically their economic effects and the casualities caused. The data ranges from 1950 to 2011, and includes many different weather events.
More information can be found at https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2Fpd01016005curr.pdf.

## Import libraries

```r
require(dtplyr)
require(ggplot2)
require(xtable)
```

## Data Processing
First, the data is downloaded, then read into a data table. read.csv reads directly from the compressed file.

```r
## Set local filename for dataset
filename <- 'StormData.csv.bz2'

## Download and unzip the dataset
if (!file.exists(filename)) {
  fileURL <- 'https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2'
  download.file(fileURL, filename)
}

if (!exists('StormData')) StormData <- tbl_dt(read.csv(filename))
```

## Results
### Health Effects
The first analysis will be to find which events are the most destructive in terms of population health. We can look at this three ways.
First, we will look at which events cause the most fatalities.

```r
## Group sum of fatalities by event type
fatalities_by_event <- StormData[,.(fatalities=as.integer(sum(FATALITIES))), by=EVTYPE]

## Order the list by descending number of fatalities
fatalities_by_event <- fatalities_by_event[order(-fatalities)]

## Print the first 20 rows of the new table
names(fatalities_by_event) <- c('Event Type', 'Total Fatalities')
print(xtable(fatalities_by_event[1:20]), type='html')
```

<!-- html table generated in R 3.3.2 by xtable 1.8-2 package -->
<!-- Thu Feb 02 22:33:54 2017 -->
<table border=1>
<tr> <th>  </th> <th> Event Type </th> <th> Total Fatalities </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> TORNADO </td> <td align="right"> 5633 </td> </tr>
  <tr> <td align="right"> 2 </td> <td> EXCESSIVE HEAT </td> <td align="right"> 1903 </td> </tr>
  <tr> <td align="right"> 3 </td> <td> FLASH FLOOD </td> <td align="right"> 978 </td> </tr>
  <tr> <td align="right"> 4 </td> <td> HEAT </td> <td align="right"> 937 </td> </tr>
  <tr> <td align="right"> 5 </td> <td> LIGHTNING </td> <td align="right"> 816 </td> </tr>
  <tr> <td align="right"> 6 </td> <td> TSTM WIND </td> <td align="right"> 504 </td> </tr>
  <tr> <td align="right"> 7 </td> <td> FLOOD </td> <td align="right"> 470 </td> </tr>
  <tr> <td align="right"> 8 </td> <td> RIP CURRENT </td> <td align="right"> 368 </td> </tr>
  <tr> <td align="right"> 9 </td> <td> HIGH WIND </td> <td align="right"> 248 </td> </tr>
  <tr> <td align="right"> 10 </td> <td> AVALANCHE </td> <td align="right"> 224 </td> </tr>
  <tr> <td align="right"> 11 </td> <td> WINTER STORM </td> <td align="right"> 206 </td> </tr>
  <tr> <td align="right"> 12 </td> <td> RIP CURRENTS </td> <td align="right"> 204 </td> </tr>
  <tr> <td align="right"> 13 </td> <td> HEAT WAVE </td> <td align="right"> 172 </td> </tr>
  <tr> <td align="right"> 14 </td> <td> EXTREME COLD </td> <td align="right"> 160 </td> </tr>
  <tr> <td align="right"> 15 </td> <td> THUNDERSTORM WIND </td> <td align="right"> 133 </td> </tr>
  <tr> <td align="right"> 16 </td> <td> HEAVY SNOW </td> <td align="right"> 127 </td> </tr>
  <tr> <td align="right"> 17 </td> <td> EXTREME COLD/WIND CHILL </td> <td align="right"> 125 </td> </tr>
  <tr> <td align="right"> 18 </td> <td> STRONG WIND </td> <td align="right"> 103 </td> </tr>
  <tr> <td align="right"> 19 </td> <td> BLIZZARD </td> <td align="right"> 101 </td> </tr>
  <tr> <td align="right"> 20 </td> <td> HIGH SURF </td> <td align="right"> 101 </td> </tr>
   </table>

Now, we look at events that cause the most injuries. Injuries may not be as serious as fatalities, but they do have a greater drain on 
the economy, as the injured take up hospital beds, and doctors must be paid for their service.

```r
## Group sum of injuries by event type
injuries_by_event <- StormData[,.(injuries=as.integer(sum(INJURIES))), by=EVTYPE]

## Order the list by descending number of injuries
injuries_by_event <- injuries_by_event[order(-injuries)]

## Print the first 20 rows of the new table
names(injuries_by_event) <- c('Event Type', 'Total Injuries')
print(xtable(injuries_by_event[1:20]), type='html')
```

<!-- html table generated in R 3.3.2 by xtable 1.8-2 package -->
<!-- Thu Feb 02 22:33:54 2017 -->
<table border=1>
<tr> <th>  </th> <th> Event Type </th> <th> Total Injuries </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> TORNADO </td> <td align="right"> 91346 </td> </tr>
  <tr> <td align="right"> 2 </td> <td> TSTM WIND </td> <td align="right"> 6957 </td> </tr>
  <tr> <td align="right"> 3 </td> <td> FLOOD </td> <td align="right"> 6789 </td> </tr>
  <tr> <td align="right"> 4 </td> <td> EXCESSIVE HEAT </td> <td align="right"> 6525 </td> </tr>
  <tr> <td align="right"> 5 </td> <td> LIGHTNING </td> <td align="right"> 5230 </td> </tr>
  <tr> <td align="right"> 6 </td> <td> HEAT </td> <td align="right"> 2100 </td> </tr>
  <tr> <td align="right"> 7 </td> <td> ICE STORM </td> <td align="right"> 1975 </td> </tr>
  <tr> <td align="right"> 8 </td> <td> FLASH FLOOD </td> <td align="right"> 1777 </td> </tr>
  <tr> <td align="right"> 9 </td> <td> THUNDERSTORM WIND </td> <td align="right"> 1488 </td> </tr>
  <tr> <td align="right"> 10 </td> <td> HAIL </td> <td align="right"> 1361 </td> </tr>
  <tr> <td align="right"> 11 </td> <td> WINTER STORM </td> <td align="right"> 1321 </td> </tr>
  <tr> <td align="right"> 12 </td> <td> HURRICANE/TYPHOON </td> <td align="right"> 1275 </td> </tr>
  <tr> <td align="right"> 13 </td> <td> HIGH WIND </td> <td align="right"> 1137 </td> </tr>
  <tr> <td align="right"> 14 </td> <td> HEAVY SNOW </td> <td align="right"> 1021 </td> </tr>
  <tr> <td align="right"> 15 </td> <td> WILDFIRE </td> <td align="right"> 911 </td> </tr>
  <tr> <td align="right"> 16 </td> <td> THUNDERSTORM WINDS </td> <td align="right"> 908 </td> </tr>
  <tr> <td align="right"> 17 </td> <td> BLIZZARD </td> <td align="right"> 805 </td> </tr>
  <tr> <td align="right"> 18 </td> <td> FOG </td> <td align="right"> 734 </td> </tr>
  <tr> <td align="right"> 19 </td> <td> WILD/FOREST FIRE </td> <td align="right"> 545 </td> </tr>
  <tr> <td align="right"> 20 </td> <td> DUST STORM </td> <td align="right"> 440 </td> </tr>
   </table>

Finally, we sum injuries and fatalities to look at total casualty numbers.

```r
## Group sum of injuries and fatalities by event type
casualties_by_event <- StormData[,.(casualties=as.integer(sum(INJURIES)+sum(FATALITIES))), by=EVTYPE]

## Order the list by descending number of injuries
casualties_by_event <- casualties_by_event[order(-casualties)]

## Print the first 20 rows of the new table
names(casualties_by_event) <- c('Event Type', 'Total Casualties')
print(xtable(casualties_by_event[1:20]), type='html')
```

<!-- html table generated in R 3.3.2 by xtable 1.8-2 package -->
<!-- Thu Feb 02 22:33:54 2017 -->
<table border=1>
<tr> <th>  </th> <th> Event Type </th> <th> Total Casualties </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> TORNADO </td> <td align="right"> 96979 </td> </tr>
  <tr> <td align="right"> 2 </td> <td> EXCESSIVE HEAT </td> <td align="right"> 8428 </td> </tr>
  <tr> <td align="right"> 3 </td> <td> TSTM WIND </td> <td align="right"> 7461 </td> </tr>
  <tr> <td align="right"> 4 </td> <td> FLOOD </td> <td align="right"> 7259 </td> </tr>
  <tr> <td align="right"> 5 </td> <td> LIGHTNING </td> <td align="right"> 6046 </td> </tr>
  <tr> <td align="right"> 6 </td> <td> HEAT </td> <td align="right"> 3037 </td> </tr>
  <tr> <td align="right"> 7 </td> <td> FLASH FLOOD </td> <td align="right"> 2755 </td> </tr>
  <tr> <td align="right"> 8 </td> <td> ICE STORM </td> <td align="right"> 2064 </td> </tr>
  <tr> <td align="right"> 9 </td> <td> THUNDERSTORM WIND </td> <td align="right"> 1621 </td> </tr>
  <tr> <td align="right"> 10 </td> <td> WINTER STORM </td> <td align="right"> 1527 </td> </tr>
  <tr> <td align="right"> 11 </td> <td> HIGH WIND </td> <td align="right"> 1385 </td> </tr>
  <tr> <td align="right"> 12 </td> <td> HAIL </td> <td align="right"> 1376 </td> </tr>
  <tr> <td align="right"> 13 </td> <td> HURRICANE/TYPHOON </td> <td align="right"> 1339 </td> </tr>
  <tr> <td align="right"> 14 </td> <td> HEAVY SNOW </td> <td align="right"> 1148 </td> </tr>
  <tr> <td align="right"> 15 </td> <td> WILDFIRE </td> <td align="right"> 986 </td> </tr>
  <tr> <td align="right"> 16 </td> <td> THUNDERSTORM WINDS </td> <td align="right"> 972 </td> </tr>
  <tr> <td align="right"> 17 </td> <td> BLIZZARD </td> <td align="right"> 906 </td> </tr>
  <tr> <td align="right"> 18 </td> <td> FOG </td> <td align="right"> 796 </td> </tr>
  <tr> <td align="right"> 19 </td> <td> RIP CURRENT </td> <td align="right"> 600 </td> </tr>
  <tr> <td align="right"> 20 </td> <td> WILD/FOREST FIRE </td> <td align="right"> 557 </td> </tr>
   </table>

### Economic Effects
This is a tricky thing to analyze. In examining the data, I've discovered what can only be OCR errors in the data. The PROPDMGEXP field 
contains a letter that represents the multiplication of PROPDMG (e.g. K=1,000, M=1,000,000...). However, there are some odd values recorded. 
For example, there are 28 instances of '5' appearing in the PROPDMGEXP field. After some digging, I found that the OCR was scanning 
to the first digit after the decimal point, then putting the next character in the PROPDMGEXP field, so a '2.5' in the PROPDMG field, 
and a '5' in the PROPDMGEXP field would represent 2.55 times some now unknown multiplier. Another peculiarity is the 'H' multiplier. 
This is not an OCR error, as it shows in the original document, but there is no explanation of what it means (My guess is it means 100). 
My solution here will be to remove all values except those with a DMGEXP (PROP, and CROP) of 'B', 'K', 'm', and 'M'.

First we'll take a look at just property damage.

```r
PropDmg <- StormData[PROPDMGEXP %in% c('B', 'K', 'm', 'M')]
PropDmg <- PropDmg[PROPDMGEXP=='m', PROPDMGEXP:='M']
PropDmg$PROPDMGEXP <- factor(PropDmg$PROPDMGEXP, levels=c('K', 'M', 'B'), labels=c(1000, 1000000, 1000000000))
PropDmg <- PropDmg[ ,PROPDMGTOT := PROPDMG * as.numeric(levels(PROPDMGEXP))[PROPDMGEXP]]

PropDmg_by_event <- PropDmg[, .(TotPropDmg=sum(PROPDMGTOT)), by=EVTYPE]
PropDmg_by_event <- PropDmg_by_event[order(-TotPropDmg)]
PropDmg_by_event <- PropDmg_by_event[,TotPropDmgStr:=paste0('$', formatC(TotPropDmg, format="f", digits=2, big.mark=","))]
names(PropDmg_by_event) <- c('Event Type', 'TotPropDmg', 'Total Property Damage')

print(xtable(PropDmg_by_event[1:20, .(`Event Type`, `Total Property Damage`)]), type='html')
```

<!-- html table generated in R 3.3.2 by xtable 1.8-2 package -->
<!-- Thu Feb 02 22:33:55 2017 -->
<table border=1>
<tr> <th>  </th> <th> Event Type </th> <th> Total Property Damage </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> FLOOD </td> <td> $144,657,709,800.00 </td> </tr>
  <tr> <td align="right"> 2 </td> <td> HURRICANE/TYPHOON </td> <td> $69,305,840,000.00 </td> </tr>
  <tr> <td align="right"> 3 </td> <td> TORNADO </td> <td> $56,937,160,480.00 </td> </tr>
  <tr> <td align="right"> 4 </td> <td> STORM SURGE </td> <td> $43,323,536,000.00 </td> </tr>
  <tr> <td align="right"> 5 </td> <td> FLASH FLOOD </td> <td> $16,140,811,510.00 </td> </tr>
  <tr> <td align="right"> 6 </td> <td> HAIL </td> <td> $15,732,266,720.00 </td> </tr>
  <tr> <td align="right"> 7 </td> <td> HURRICANE </td> <td> $11,868,319,010.00 </td> </tr>
  <tr> <td align="right"> 8 </td> <td> TROPICAL STORM </td> <td> $7,703,890,550.00 </td> </tr>
  <tr> <td align="right"> 9 </td> <td> WINTER STORM </td> <td> $6,688,497,250.00 </td> </tr>
  <tr> <td align="right"> 10 </td> <td> HIGH WIND </td> <td> $5,270,046,260.00 </td> </tr>
  <tr> <td align="right"> 11 </td> <td> RIVER FLOOD </td> <td> $5,118,945,500.00 </td> </tr>
  <tr> <td align="right"> 12 </td> <td> WILDFIRE </td> <td> $4,765,114,000.00 </td> </tr>
  <tr> <td align="right"> 13 </td> <td> STORM SURGE/TIDE </td> <td> $4,641,188,000.00 </td> </tr>
  <tr> <td align="right"> 14 </td> <td> TSTM WIND </td> <td> $4,484,928,440.00 </td> </tr>
  <tr> <td align="right"> 15 </td> <td> ICE STORM </td> <td> $3,944,927,810.00 </td> </tr>
  <tr> <td align="right"> 16 </td> <td> THUNDERSTORM WIND </td> <td> $3,483,121,140.00 </td> </tr>
  <tr> <td align="right"> 17 </td> <td> HURRICANE OPAL </td> <td> $3,172,846,000.00 </td> </tr>
  <tr> <td align="right"> 18 </td> <td> WILD/FOREST FIRE </td> <td> $3,001,829,500.00 </td> </tr>
  <tr> <td align="right"> 19 </td> <td> HEAVY RAIN/SEVERE WEATHER </td> <td> $2,500,000,000.00 </td> </tr>
  <tr> <td align="right"> 20 </td> <td> THUNDERSTORM WINDS </td> <td> $1,735,952,850.00 </td> </tr>
   </table>

Next, we can look at crop damage.

```r
CropDmg <- StormData[CROPDMGEXP %in% c('B', 'k', 'K', 'm', 'M')]
CropDmg <- CropDmg[CROPDMGEXP=='m', CROPDMGEXP:='M']
CropDmg <- CropDmg[CROPDMGEXP=='k', CROPDMGEXP:='K']
CropDmg$CROPDMGEXP <- factor(CropDmg$CROPDMGEXP, levels=c('K', 'M', 'B'), labels=c(1000, 1000000, 1000000000))
CropDmg <- CropDmg[ ,CROPDMGTOT := CROPDMG * as.numeric(levels(CROPDMGEXP))[CROPDMGEXP]]

CropDmg_by_event <- CropDmg[, .(TotCropDmg=sum(CROPDMGTOT)), by=EVTYPE]
CropDmg_by_event <- CropDmg_by_event[order(-TotCropDmg)]
CropDmg_by_event <- CropDmg_by_event[,TotCropDmgStr:=paste0('$', formatC(TotCropDmg, format="f", digits=2, big.mark=","))]
names(CropDmg_by_event) <- c('Event Type', 'TotCropDmg', 'Total Crop Damage')

print(xtable(CropDmg_by_event[1:20, .(`Event Type`, `Total Crop Damage`)]), type='html')
```

<!-- html table generated in R 3.3.2 by xtable 1.8-2 package -->
<!-- Thu Feb 02 22:33:55 2017 -->
<table border=1>
<tr> <th>  </th> <th> Event Type </th> <th> Total Crop Damage </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> DROUGHT </td> <td> $13,972,566,000.00 </td> </tr>
  <tr> <td align="right"> 2 </td> <td> FLOOD </td> <td> $5,661,968,450.00 </td> </tr>
  <tr> <td align="right"> 3 </td> <td> RIVER FLOOD </td> <td> $5,029,459,000.00 </td> </tr>
  <tr> <td align="right"> 4 </td> <td> ICE STORM </td> <td> $5,022,113,500.00 </td> </tr>
  <tr> <td align="right"> 5 </td> <td> HAIL </td> <td> $3,025,954,450.00 </td> </tr>
  <tr> <td align="right"> 6 </td> <td> HURRICANE </td> <td> $2,741,910,000.00 </td> </tr>
  <tr> <td align="right"> 7 </td> <td> HURRICANE/TYPHOON </td> <td> $2,607,872,800.00 </td> </tr>
  <tr> <td align="right"> 8 </td> <td> FLASH FLOOD </td> <td> $1,421,317,100.00 </td> </tr>
  <tr> <td align="right"> 9 </td> <td> EXTREME COLD </td> <td> $1,292,973,000.00 </td> </tr>
  <tr> <td align="right"> 10 </td> <td> FROST/FREEZE </td> <td> $1,094,086,000.00 </td> </tr>
  <tr> <td align="right"> 11 </td> <td> HEAVY RAIN </td> <td> $733,399,800.00 </td> </tr>
  <tr> <td align="right"> 12 </td> <td> TROPICAL STORM </td> <td> $678,346,000.00 </td> </tr>
  <tr> <td align="right"> 13 </td> <td> HIGH WIND </td> <td> $638,571,300.00 </td> </tr>
  <tr> <td align="right"> 14 </td> <td> TSTM WIND </td> <td> $554,007,350.00 </td> </tr>
  <tr> <td align="right"> 15 </td> <td> EXCESSIVE HEAT </td> <td> $492,402,000.00 </td> </tr>
  <tr> <td align="right"> 16 </td> <td> FREEZE </td> <td> $446,225,000.00 </td> </tr>
  <tr> <td align="right"> 17 </td> <td> TORNADO </td> <td> $414,953,110.00 </td> </tr>
  <tr> <td align="right"> 18 </td> <td> THUNDERSTORM WIND </td> <td> $414,843,050.00 </td> </tr>
  <tr> <td align="right"> 19 </td> <td> HEAT </td> <td> $401,461,500.00 </td> </tr>
  <tr> <td align="right"> 20 </td> <td> WILDFIRE </td> <td> $295,472,800.00 </td> </tr>
   </table>

Last, we examine total damages.

```r
TotDmg <- PropDmg_by_event[CropDmg_by_event, on='Event Type']
TotDmg <- TotDmg[, TotDamages := TotPropDmg + TotCropDmg]
TotDmg <- TotDmg[order(-TotDamages)]
TotDmg <- TotDmg[, 'Total Damages' := paste0('$', formatC(TotDamages, format="f", digits=2, big.mark=","))]

print(xtable(TotDmg[1:20, .(`Event Type`, `Total Damages`)]), type='html')
```

<!-- html table generated in R 3.3.2 by xtable 1.8-2 package -->
<!-- Thu Feb 02 22:33:55 2017 -->
<table border=1>
<tr> <th>  </th> <th> Event Type </th> <th> Total Damages </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> FLOOD </td> <td> $150,319,678,250.00 </td> </tr>
  <tr> <td align="right"> 2 </td> <td> HURRICANE/TYPHOON </td> <td> $71,913,712,800.00 </td> </tr>
  <tr> <td align="right"> 3 </td> <td> TORNADO </td> <td> $57,352,113,590.00 </td> </tr>
  <tr> <td align="right"> 4 </td> <td> STORM SURGE </td> <td> $43,323,541,000.00 </td> </tr>
  <tr> <td align="right"> 5 </td> <td> HAIL </td> <td> $18,758,221,170.00 </td> </tr>
  <tr> <td align="right"> 6 </td> <td> FLASH FLOOD </td> <td> $17,562,128,610.00 </td> </tr>
  <tr> <td align="right"> 7 </td> <td> DROUGHT </td> <td> $15,018,672,000.00 </td> </tr>
  <tr> <td align="right"> 8 </td> <td> HURRICANE </td> <td> $14,610,229,010.00 </td> </tr>
  <tr> <td align="right"> 9 </td> <td> RIVER FLOOD </td> <td> $10,148,404,500.00 </td> </tr>
  <tr> <td align="right"> 10 </td> <td> ICE STORM </td> <td> $8,967,041,310.00 </td> </tr>
  <tr> <td align="right"> 11 </td> <td> TROPICAL STORM </td> <td> $8,382,236,550.00 </td> </tr>
  <tr> <td align="right"> 12 </td> <td> WINTER STORM </td> <td> $6,715,441,250.00 </td> </tr>
  <tr> <td align="right"> 13 </td> <td> HIGH WIND </td> <td> $5,908,617,560.00 </td> </tr>
  <tr> <td align="right"> 14 </td> <td> WILDFIRE </td> <td> $5,060,586,800.00 </td> </tr>
  <tr> <td align="right"> 15 </td> <td> TSTM WIND </td> <td> $5,038,935,790.00 </td> </tr>
  <tr> <td align="right"> 16 </td> <td> STORM SURGE/TIDE </td> <td> $4,642,038,000.00 </td> </tr>
  <tr> <td align="right"> 17 </td> <td> THUNDERSTORM WIND </td> <td> $3,897,964,190.00 </td> </tr>
  <tr> <td align="right"> 18 </td> <td> HURRICANE OPAL </td> <td> $3,191,846,000.00 </td> </tr>
  <tr> <td align="right"> 19 </td> <td> WILD/FOREST FIRE </td> <td> $3,108,626,330.00 </td> </tr>
  <tr> <td align="right"> 20 </td> <td> THUNDERSTORM WINDS </td> <td> $1,926,607,550.00 </td> </tr>
   </table>

