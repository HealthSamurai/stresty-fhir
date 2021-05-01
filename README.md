[![Stresty FHIR](https://github.com/HealthSamurai/stresty-fhir/actions/workflows/main.yml/badge.svg)](https://github.com/HealthSamurai/stresty-fhir/actions/workflows/main.yml)

## FHIR scripts for stresty

The repository is a collection of FHIR test suits for https://github.com/HealthSamurai/stresty.

The collection is organized by FHIR versions and well-known FHIR scenarios.

https://healthsamurai.github.io/stresty-fhir/

## Requirements

- Install Stresty utility into your local environment
- Have some text editor supporting edn syntax (Try Sublime (https://www.sublimetext.com/)  for example)

## Installation & Configuration

- Download the latest version of Stresty utility here https://github.com/HealthSamurai/stresty/releases/

MAC OS / Linux environment

Add sty to $PATH

``` bash
mkdir bin
cp {you sty file location}\sty bin
cd bin
export PATH=`pwd`:$PATH
```

In your terminal make sty utility executable by executing

``` bash
chmod -x sty
```


Generating the new project

``` bash
sty gen --project=mytest   //mytest - is a name of the namespace
```

MAC OS

Follow this instruction to handle System restrictions for the apps from unidentified developers: https://www.macworld.co.uk/how-to/mac-app-unidentified-developer-3669596/#:~:text=Open%20System%20Preferences.,developers'%20from%20just%20App%20Store.


## sty utility usage

``` bash
sty --path=PATH <command> <subcommand>

sty server -p PORT

sty tests --envs=ENV1,ENV2 --cases=CS,CS  --steps=STEP,STEP --tags=TAG,TAG

sty watch --paths=PATH1,PATH2

sty (help) => help
```

## Create new namespace for test cases

Create new .edn file or add some additional workspaces to servers.edn

``` edn
{ns fhirserver                  //the namespace name
 import #{sty fhir.mytest} // the list of test suits

 fhirserver                    // FHIR server title and connection settings
 {:zen/tags #{sty/env}
  :base-url "https://edgeniquola.edge.aidbox.app"
  :basic-auth {:user "basic" :password "secret"}}
```

mytest.edn

``` edn
{ns mytest
 import #{sty}

 env
 {:zen/tags #{sty/env}
  :base-url "https://edgeniquola.edge.aidbox.app"
  :basic-auth {:user "???" :password "???"}}

 case
 {:zen/tags #{sty/case}
  :title "Patient track"
  :steps
  [{:id :create
    :desc  "create"
    :do {:act sty/http
         :method :post
         :url "/Patient"
         :body {:resourceType "Patient"}}
    :match {:by sty/matcho
            :status 201
            :body {:meta {:lastUpdated sty/string?
                          :versionId sty/string?}}}}

   {:id :read
    :desc  "read"
    :do {:act sty/http
         :method :get
         :url (str "/Patient/" (get-in sty/state [:create :body :id]))}
    :match {:by sty/matcho
            :status sty/ok?
            :body {:id (get-in sty/state [:create :body :id])
                   :meta {:lastUpdated sty/string?
                          :versionId sty/string?}}}}

   {:id :read-2
    :desc  "wrong read"
    :do {:act sty/http
         :method :get
         :url (str "/Patient/" (get-in sty/state [:create :body :id]))}
    :match {:by sty/matcho
            :status sty/ok?
            :body {:id (str "UPS-" (get-in sty/state [:create :body :id]))}}}
   ]
  }
 }
```


## List of supported options for test case

https://github.com/HealthSamurai/stresty/blob/master/src/sty.edn

## Run tests

```
> sty --ns=mytest tests

```
