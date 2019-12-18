Welcome to the open_hub developer documentation.

## GitFlow
This project used GitFlow [methodology](https://datasift.github.io/gitflow/IntroducingGitFlow.html). All developers final commit is to develop branch. Master branch is lock to team lead only. From there, team lead deploy to production environment.


## Common building block
Following are the common building blocks of Central. Central keep track of:

  * [Organization](Organization) - think startup, company, corp, gov agency...
  * [Product](Product)
  * [Individual](Individual) - think founder, participant, point of contact...
  * [Event](Event) - think workshop, sharing, talks, classes, gathering...

## Default Modules
These are the modules bundle with the package.
  * [Journey](Journey)
  * [F7](F7 Form)
  * [Comment](Comment)
  * [Currency](Currency)
  * [esLog](esLog) (application log using Elastic Search)
  * [Registry](Registry)
  * [Request](Request)
  * [Cache](Cache)
  * [Junk](Junk)

## Master Data

  * Product Category
  * Cluster
  * [Persona](Persona)
  * Industry
  * Impact
  * SDG
  * [Startup Stages](Startup Stages)
  * Legal Form
  * Country
  * State
  * City


## Topics

* [API for Developers](API4Developers)
* [Modules Architecture](ModulesArchitecture)
* [Multi Domains](MultiDomains)
* [Multi Lingual i18n](i18n)
