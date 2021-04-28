## FHIR scripts for stresty

The repository is a collection of FHIR test suits for https://github.com/HealthSamurai/stresty.

The collection is organized by FHIR versions and well-known FHIR scenarios.

## Requirements

- Install Stresty utility into your local environment
- Have some text editor supporting edn syntax (Try Sublime for example)
- 

## Installation & Configuration

- Download the latest version of Stresty utility here https://github.com/HealthSamurai/stresty/releases/ 

MAC OS

Add sty to $PATH

```
mkdir bin
cp {you sty file location}\sty bin
cd bin
export PATH=`pwd`:$PATH
```

In your terminal make sty utility executable by executing

```
chmod -x sty
```


Generating the new project

```
sty gen —project=my.namespace
```

Disabling the built-in MAC OS security for unknown app publishers.
Open System Preferencies -> Security&Privacy -> Allow apps downloaded from -> "Allow anyway"



Linux



## Writing tests

```
# run tests
sty aidbox

```
