[![Stresty FHIR](https://github.com/HealthSamurai/stresty-fhir/actions/workflows/main.yml/badge.svg)](https://github.com/HealthSamurai/stresty-fhir/actions/workflows/main.yml)

## FHIR scripts for stresty

The repository is a collection of FHIR test suits for [Stresty](https://github.com/HealthSamurai/stresty).

The collection is organized by FHIR versions and well-known FHIR scenarios.

[CI test runner report](https://healthsamurai.github.io/stresty-fhir/)

## Requirements

- Install Stresty utility into your local environment
- Have some text editor supporting edn syntax (Try [Sublime](https://www.sublimetext.com/)  for example)

## Installation & Configuration

- Download the latest version of [Stresty](https://github.com/HealthSamurai/stresty/releases/)

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

Follow this [instruction](https://www.macworld.co.uk/how-to/mac-app-unidentified-developer-3669596/#:~:text=Open%20System%20Preferences.,developers'%20from%20just%20App%20Store) to handle System restrictions for the apps from unidentified developers.


## sty utility usage

``` bash
sty --path=PATH <command> <subcommand>

sty server -p PORT

sty tests --envs=ENV1,ENV2 --cases=CS,CS  --steps=STEP,STEP --tags=TAG,TAG

sty watch --paths=PATH1,PATH2

sty (help) => help
```

## Working with environments for running test cases

New generated project contains envs.edn file which specifies a list of FHIR servers to run your tests

The envs file structure is

``` edn
ns envs               //the environment name
 import #{
   sty
   mytest.case        // the list of test suits
 }

 env {               // FHIR server title and connection settings
  :zen/tags #{sty/env}
  :base-url "???"
  :basic-auth {:user "???" :password "???"}
 }

}
```

If you would like to easily run the tests on your local instance of the FHIR server, you can just [install Devbox](https://docs.aidbox.app/getting-started/installation/setup-aidbox.dev)


## Writing test cases

mytest.edn

``` edn
{ns mytest.case
import #{sty}

case
 {:zen/tags #{sty/case}
  :steps [
  {:id :first-step
   :desc "Crate Patient"
   :do {:act sty/http 
        :method :post
        :url "/Patient"
        :body {:resourceType "Patient"
               :name [{:family "Doe", :given ["John"]}]}}
   :match {:by sty/matcho
           :status sty/ok?
           :body {:id sty/string?}}}

  {:id :second-step
   :do {:act sty/print
        :path [:first-step :body]}}

  {:id :third-step
   :do {:act sty/http 
        :method :get
        :url (str "/Patient/" (get-in sty/state [:first-step :body :id]))}
   :match {:by sty/matcho
           :status sty/ok?
           :body {:name (get-in sty/state [:first-step :body :name])}}}


  {:id :fourth-step
   :do {:act sty/http 
        :method :get
        :url (str "/Patient/" (get-in sty/state [:first-step :body :id]))}
   :match {:by sty/matcho
           :status sty/ok?
           :body {:name [{:family sty/any?}]}}}

  ;; add more steps
  ]}}
```


Supported predicates for a test case


```
string? 

number? 

integer? 

ok? 

any? 

```

## Runing tests

```
sty --ns=envs tests

```
