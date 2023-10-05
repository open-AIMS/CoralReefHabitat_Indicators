# Coral Indicators
by Manuel Gonzalez Rivero, Angus Thompson, Kerryn Crossman, Murray Logan

# About

# Dependencies

Although this codebase has been tested and runs on a 2023 R runtime
environment, there is no guarantee that underlying changes to either R
or the necessary R packages will preserve the stability of these
codes.  As a result, it is strongly recommended that these codes be
run via _containerisation_.



# Building the docker container image

It the underlying system contains the `make` tools, a Docker image can
be generated via the following:

```{build docker, engine='bash', results='markdown', eval=FALSE}
make build
```

Alternatively, the following command can be issued in a terminal
within the project root folder.

```{build docker alt, engine='bash', results='markdown', eval=FALSE}
docker build . --tag coral_indicators
```

Either way, you can then confirm that the Docker image has been built
by looking at a list of all Docker images on your local machine.

```{build docker images, engine='bash', results='markdown', eval=FALSE}
docker images
```

There should be a "REPOSITORY" called `coral_indicators` with a "TAG"
of `latest`.

# Running R in the docker container (interactively)

If you have the `make` tools:

```{run docker, engine='bash', results='markdown', eval=FALSE}
make R_container
```

Alternatively,

```{run docker alt, engine='bash', results='markdown', eval=FALSE}
docker run --rm -v -it "$(pwd):/home/Project" coral_indicators R
```

# Running the code in the docker container

If you have the `make` tools:

```{run docker code container, engine='bash', results='markdown', eval=FALSE}
make code_container
```

Alternatively with more control:

- final year of 2023 (a final year just mean include all data up to
  and including the year 2023)
- remove all intermediate and final data products to ensure that the
  current run is not polluted by previous runs (if they have occured).
- do not re-run baseline models

```{run docker code container alt, engine='bash', results='markdown', eval=FALSE}
docker run --rm -v -it "$(pwd):/home/Project" coral_indicators Rscript R/00_main.R --final_year=2023 --fresh_start=true --rerun_baselines=false
```

Alternatively, running a single stage

- final year of 2023 (a final year just mean include all data up to
  and including the year 2023)
- remove all intermediate and final data products to ensure that the
  current run is not polluted by previous runs (if they have occured).
- only run stage 7 (compositional model)
- do not re-run baseline models

```{run docker code container alt2, engine='bash', results='markdown', eval=FALSE}
docker run --rm -v -it "$(pwd):/home/Project" coral_indicators Rscript R/00_main.R --final_year=2023 --fresh_start=true --runStage=7 --rerun_baselines=false
```

# Compiling documents in the docker container

If you have the `make` tools:

```{run docker docs container, engine='bash', results='markdown', eval=FALSE}
make docs_container
```

Alternatively,

```{run docker docs container alt, engine='bash', results='markdown', eval=FALSE}
docker run --rm -v -it "$(pwd):/home/Project" coral_indicators Rscript docs/00_main.Rmd
```

# Running the code locally 

If you have the `make` tools:

```{run docker code local, engine='bash', results='markdown', eval=FALSE}
make code_local
```

# Compiling the documents locally 

If you have the `make` tools:

```{run docker docs local, engine='bash', results='markdown', eval=FALSE}
make docs_local
```

# Building singularity

```{build singularity alt, engine='bash', results='markdown', eval=FALSE}
docker save coral_indicators -o coral_indicators.tar 
singularity build coral_indicators.sif docker-archive://coral_indicators.tar
```

# Running (executing) singularity

The following illustrates how to run via singularity with the following settings:

- final year of 2023 (a final year just mean include all data up to
  and including the year 2023)
- remove all intermediate and final data products to ensure that the
  current run is not polluted by previous runs (if they have occured).
- do not re-run baseline models

```{run singularity alt, engine='bash', results='markdown', eval=FALSE}
singularity exec -B .:/home/Project coral_indicators.sif Rscript 00_main.R --final_year=2023 --fresh_start=true --rerun_baselines=false
```

# Running (executing) a single stage in singularity

The following illustrates how to run via singularity with the following settings:

- final year of 2023 (a final year just mean include all data up to
  and including the year 2023)
- remove all intermediate and final data products to ensure that the
  current run is not polluted by previous runs (if they have occured).
- only run stage 7 (compositional model)
- do not re-run baseline models

```{run singularity alt2, engine='bash', results='markdown', eval=FALSE}
singularity exec -B .:/home/Project coral_indicators.sif Rscript 00_main.R --final_year=2023 --fresh_start=true --runStage=7 --rerun_baselines=false
```
