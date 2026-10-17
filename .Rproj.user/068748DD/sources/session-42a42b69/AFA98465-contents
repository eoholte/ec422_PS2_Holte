library(tidyquant)
library(tidyverse)

# unemp_raw <- tq_get(
#   c("UNRATE", "ORUR", "CAUR", "TXUR", "NYUR"),
#   get  = "economic.data",
#   from = "1976-01-01"
# )
unemp_raw = read_rds("data/unemp_raw.rds")
head(unemp_raw)

dir.create("data", showWarnings = FALSE)

write_rds(unemp_raw, "data/unemp_raw.rds")

unemp <- unemp_raw |>
  rename(unemployment_rate = price) |>
  mutate(
    region = case_when(
      symbol == "UNRATE" ~ "United States",
      symbol == "ORUR" ~ "Oregon",
      symbol == "CAUR" ~ "California",
      symbol == "TXUR" ~ "Texas",
      symbol == "NYUR" ~ "New York"
    )
  )
head(unemp)

unemp <- unemp |>
  arrange(region, date) |>
  group_by(region) |>
  mutate(
    unemployment_rate = replace(
      unemployment_rate,
      date == as.Date("2025-10-01"),
      unemployment_rate[date == as.Date("2025-09-01")]
    )
  ) |>
  ungroup()

unemp <- unemp |>
  arrange(region, date) |>
  group_by(region) |>
  mutate(unemployment_rate_change = unemployment_rate - lag(unemployment_rate)) |>
  ungroup()

unemp_top_increases <- unemp |>
  slice_max(unemployment_rate_change, n=5)

unemp_top_decreases <- unemp |>
  slice_min(unemployment_rate_change, n=5)

unemp_no_2020 <- unemp |>
  filter(lubridate::year(date) != 2020)

unemp_top_increases_no_covid <- unemp_no_2020 |>
  slice_max(unemployment_rate_change, n=5)

unemp_top_decreases_no_covid <- unemp_no_2020 |>
  slice_min(unemployment_rate_change, n=5)

