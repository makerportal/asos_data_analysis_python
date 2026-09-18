# ASOS Data Analysis in Python

Monthly-averaged diurnal temperature variability for a single ASOS surface
station, from a raw NOAA/NCEI `.dat` file — an introduction to `numpy` and
`matplotlib` on data that has real gaps in it.

![Hourly averaged temperature with standard deviation](asos_analysis_python.png)

## Getting the data

The script reads `64060KLGA201807.dat` — station **KLGA** (LaGuardia, New York),
**July 2018**, in NCEI's ASOS 5-minute (DSI-6406) format. **The file is not in
this repository**; download the month you want from NOAA/NCEI and either keep
the same name or edit the filename at the top of `monthly_asos_avg.py`.

## Running it

```
pip3 install numpy matplotlib
python3 monthly_asos_avg.py
```

It writes `asos_analysis_python.png` at 200 dpi and opens the plot.

## What the script does, and the two choices in it worth knowing

1. **It keeps only rows whose third field is `NP`** and skips everything else.
   That field is a record flag, and `NP` selects the common case; confirm its
   exact meaning against NCEI's DSI-6406 format documentation before reusing
   this parser for anything precipitation-related. Whatever it denotes, the
   average below is conditioned on it rather than taken over every record.
2. **The timestamp is parsed by character offset, not by delimiter**
   (`[3:7]` year, `[7:9]` month, `[9:11]` day, `[11:13]` hour, `[13:15]`
   minute). That is what makes a fixed-width record readable in three lines,
   and it is also what breaks silently if you feed it a different product with
   a different column layout. Check one line by hand before trusting a month.

Then it is plain `numpy`: bucket the observations by `hour`, take `np.mean` and
`np.std` per bucket, and shade ±1 standard deviation with
`plt.fill_between`. The scatter underneath is every 5-minute observation in the
month, which is what makes the spread legible rather than asserted.

---

Part of the [Maker Portal](https://makerportal.ai) open-source scientific computing and hardware ecosystem. Explore interactive calculators and engineering tools at [makerportal.ai](https://makerportal.ai).
