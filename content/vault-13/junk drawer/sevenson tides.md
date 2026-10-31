### links

map: https://tidesandcurrents.noaa.gov/map/index.html?id=8639348
sample station: https://tidesandcurrents.noaa.gov/stationhome.html?id=8639348
web services: https://tidesandcurrents.noaa.gov/web_services_info.html
api docs: https://api.tidesandcurrents.noaa.gov/api/prod/#requestResponse
sample python notebook: https://github.com/NOAA-CO-OPS/CORA-Coastal-Ocean-Reanalysis-CORA/blob/main/CORA_Accessing_Data.ipynb

### job specific info

station: https://tidesandcurrents.noaa.gov/noaatidepredictions.html?id=8530586&legacy=1
station id: ~~**8530586** (Fish Creek)~~ **8518750** (The Battery)
project: 1324 - Berry's Creek Study Area (BCSA) Phase 1- Remedial Action

Update 6/16/25: Turns out the Fish Creek station was closed in the 70s, I think the closest station is in Battery Park. Also, I think they actually want _prediction_ data, or at least I hope they do.

Update 6/19/25: *Actually*, they *do* want to use Fish Creek, and just use the predictions, which they still give you.
### objective

Display the four high and low tides w/ time & height under other report metadata (e.g., date, weather) at page top.

### building api call
##### root
`https://api.tidesandcurrents.noaa.gov/api/prod/datagetter?`
##### station
`station=[project.tideStation]`, e.g. `station=8530586`
##### date
`begin_date=[report.date]&range=24`, e.g. `begin_date=20250526&range=24`
##### data products
~~`product=high_low`~~
`product=predictions`
##### interval
Interval can be specified for `predictions` product only, sadly.
`interval=hilo`
##### datum
`datum=MLLW`
##### units
I'm not sure if this might have a default? (Update: it does not, all the fields are required.)
`units=english`
##### time zone
`time_zone=lst_ldt`
##### application
> This parameter provides an “identifier” in automated activity / error logs that allows us to identify your query from others.

`application=reports@sevensonapps.com`
##### format
`format=json`
##### all together
`https://api.tidesandcurrents.noaa.gov/api/prod/datagetter?station=8530586&begin_date=20250615&range=24&product=predictions&interval=hilo&datum=MLLW&time_zone=lst_ldt&units=english&application=reports@sevensonapps.com&format=json`

### Sample response

```json
{
  "predictions": [
    { "t": "2025-06-15 05:57", "v": "0.224", "type": "L" },
    { "t": "2025-06-15 11:59", "v": "4.121", "type": "H" },
    { "t": "2025-06-15 17:48", "v": "0.896", "type": "L" },
    { "t": "2025-06-15 23:53", "v": "4.999", "type": "H" }
  ]
}
```