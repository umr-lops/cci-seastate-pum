# Versions of 20Hz files for Jason-3

When we wanted to check that we had the same number of SGDR files as retracked 20Hz files, we realized that there were inconsistencies for Jason-3.
Indeed, we had more 20Hz files than SGDR files.
It turns out there was a mix between version D and version F, there were duplicates.
Then, it was necessary to determine a date from which version F began.

In 2016 and 2017, there is version F which fills holes in version D.
In 2019, for day 43 and day 126, there is also a combination of version F with version D in order to fill in missing files.
However, starting from day 127, there are periods where there are duplicate versions.
Indeed, from day 132 to 152, we have version D in duplicate compared to version F. 
It was then necessary to delete these duplicates in version D, in order to keep only version F, which then begins on day 127 of 2019.

