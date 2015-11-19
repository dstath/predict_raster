#
# calculate regression in a raster stack (time series)
# and then predict.
#
require(raster)
require(rgdal)

# get the list of rasters in the folder
rasters <- list.files(pattern='*.tif$')

# make the raster stack
s <- stack(rasters)

# crop the stack to the extent of Sicily
sicily_ext <- c(12, 16, 36.5, 38.5)
sicily <- crop(s, sicily_ext)

# make a time variable (to be used in regression)
time <- 1:nlayers(s)

# run the regression
fun <- function(x) { if (is.na(x[1])){ NA } else {lm(x ~ time)$coefficients }} #  $coefficients[2]

#lm(sicily ~ time)$coefficients[2]

#predict to a raster
predicted <- predict(sicily, x2, progress='text')

# END
