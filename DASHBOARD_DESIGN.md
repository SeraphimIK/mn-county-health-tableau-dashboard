# Minnesota County Health Dashboard — Design Specification

A Tableau-ready dataset and full dashboard design for visualizing health outcome measures across Minnesota's 87 counties.

## Honest note on what this is

I don't have Tableau Desktop available in the environment I built this in, so rather than falsely claim a finished `.twbx` workbook, this project is the data preparation and a complete, specific dashboard design, exactly the worksheets, fields, and layout, ready to actually build in Tableau Public in about 15-20 minutes. This uses the same real dataset as the SQL and Python projects in this portfolio.

## Data file

`mn_county_health_tableau_ready.csv`, 87 rows (one per MN county), with these fields:

- `fipscode`: 5-digit county FIPS code (use this for Tableau's built-in "County" geographic role, or plot directly with the FIPS-based map layer)
- `county`: county name
- `premature_death_rate`, `preventable_hosp_rate`, `adult_obesity_pct`, `low_birthweight_pct`: the four raw measures
- `*_percentile`: each measure's statewide percentile rank (0-100), precomputed for easy color encoding without needing a Tableau table calculation

## Worksheets to build

1. **MN Choropleth Map**: Drag `fipscode` onto the view, set its geographic role to County, then color by `premature_death_rate`. Duplicate this sheet for each of the other three measures (4 map sheets total).
2. **County Ranking Bar Chart**: Horizontal bar chart, county on rows, premature_death_rate on columns, sorted descending. Add a parameter control to let the viewer switch which measure is plotted (Parameter: "Select Measure", swapped in via a calculated field).
3. **Scatter Plot**: adult_obesity_pct on columns, preventable_hosp_rate on rows, one mark per county, county name on the Detail and Tooltip shelves.
4. **Summary Stats Text Table**: statewide average, min, and max for each measure (matches the SQL project's Query 1).

## Suggested calculated field

```
// Average Percentile
// Averages each measure's percentile rank into one number per county,
// so counties that rank high on several measures at once stand out
(([premature_death_rate_percentile] + [preventable_hosp_rate_percentile] +
  [adult_obesity_pct_percentile] + [low_birthweight_pct_percentile]) / 4)
```

Use this on the choropleth map as a fifth view to see which counties, like Mille Lacs, rank high across all four measures at once (see the SQL project in this portfolio for that finding).

## Dashboard layout

- Top: measure selector parameter control
- Left: choropleth map (updates based on selected measure)
- Right: ranking bar chart (same measure)
- Bottom: scatter plot + summary stats table side by side

## How to build it

1. Open Tableau Public (or Desktop)
2. Connect to `mn_county_health_tableau_ready.csv`
3. Build the four worksheets above
4. Combine into one dashboard using the layout described
5. Publish to Tableau Public and link it from this repo's README

## Limitations

This is a design specification and prepared dataset, not a completed `.twbx` workbook, since Tableau software wasn't available in the environment this was built in.
