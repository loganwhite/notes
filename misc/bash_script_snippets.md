## How to get the date time string in a specific format?
```bash
d=`date +'%Y%m%d'`
# the variable d will be assigned a value like: 20201223
```
If you want the date printed to be tomorrow or yesterday
```bash
d=`date --date="tomorrow" +'%Y%m%d'`
d=`date --date="yesterday" +'%Y%m%d'`
```
