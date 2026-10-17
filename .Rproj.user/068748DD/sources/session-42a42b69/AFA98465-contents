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