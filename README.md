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

## sty utility usage

``` bash
sty test | gen | server | check | help

------------------------------------------------------
sty test [tag=<string,string>] [case=<string,string>] [ns=us-core.patient,us-core.encounter] [env=servers/aidbox]
 run tests
 params:
  tag=<string,string> - tags list, run only cases and envs with this tags
  case=<string,string>  - cases list, run only listed cases
  ns=us-core.patient,us-core.encounter  - namespaces list, run tests from this list
  env=servers/aidbox  - envs list, run specific enviroments

-------------------------------------------------
sty gen [project=my.project]
 generate project and cases
 params:
  project=my.project  - name of project

-------------------------------------------------
sty server [port=8080]
 run server with ui
 params:
  port=8080 - Port to start server

-------------------------------------------------
sty check [ns=us-core.patient,us-core,encounter]
 check syntax of test project
 params:
  ns=us-core.patient,us-core,encounter  - Namespaces to check

-------------------------------------------------
sty help
 stresty help
 params:

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
sty --ns=envs test

```
