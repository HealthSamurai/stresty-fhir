## FHIR scripts for stresty

The repository is a collection of FHIR test suits for https://github.com/HealthSamurai/stresty.

The collection is organized by FHIR versions and well-known FHIR scenarios.

## Requirements

- Install Stresty utility into your local environment
- Have some text editor supporting edn syntax (Try Sublime (https://www.sublimetext.com/)  for example)

## Installation & Configuration

- Download the latest version of Stresty utility here https://github.com/HealthSamurai/stresty/releases/ 

MAC OS / Linux environment

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

MAC OS

Follow this instruction to handle System restrictions for the apps from unidentified developers: https://www.macworld.co.uk/how-to/mac-app-unidentified-developer-3669596/#:~:text=Open%20System%20Preferences.,developers'%20from%20just%20App%20Store. 




## Writing tests

```
# run tests
sty aidbox

```
