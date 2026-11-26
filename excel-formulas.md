# Excel formulas

Copied from the workbook. The member table was named `prism_plus_members` and the lookup table `city_region_lookup`.

## Region from city

```excel
=XLOOKUP(prism_plus_members[[#This Row],[city]], city_region_lookup[city], city_region_lookup[region], "CHECK")
```

`"CHECK"` instead of `#N/A` makes a city missing from the lookup easy to filter for.

## Tier order (so tiers sort Bronze to Platinum)

```excel
=IFS(prism_plus_members[[#This Row],[prism_plus_tier]]="Bronze",1,
     prism_plus_members[[#This Row],[prism_plus_tier]]="Silver",2,
     prism_plus_members[[#This Row],[prism_plus_tier]]="Gold",3,
     prism_plus_members[[#This Row],[prism_plus_tier]]="Platinum",4)
```

## Registration year

```excel
=YEAR(prism_plus_members[[#This Row],[registration_date]])
```

## Orders band

```excel
=IFS(prism_plus_members[[#This Row],[transaction_count]]<=1,"1",
     prism_plus_members[[#This Row],[transaction_count]]<=3,"2 to 3",
     prism_plus_members[[#This Row],[transaction_count]]<=9,"4 to 9",
     TRUE,"10+")
```

## Spend band

```excel
=IFS(prism_plus_members[[#This Row],[total_revenue]]<=25,"under £25",
     prism_plus_members[[#This Row],[total_revenue]]<=50,"£25 to £50",
     prism_plus_members[[#This Row],[total_revenue]]<=100,"£50 to £100",
     TRUE,"over £100")
```

## Aggregate

A PivotTable on the cleaned 3,726 rows: rows `region` then `prism_plus_tier`; values count of members, average of `transaction_count`, average of `total_revenue`. Copied as values to an `Aggregate` sheet, where every cell with fewer than 10 members was replaced with `<10` and its averages with `suppressed`. Grand totals were turned off.
