## Organization
Organizations are company or team likes startups, social enterprise, corporate, government agency, or even a temporary team out of weekend hackathon. 

Companies in the startup ecosystem are volatile. They formed over a weekend and may growth to become a unicorn in a year or might die off in a month. They are not register with the authority, hence is challenging to track.

  * [Persona](Master-Data-Persona) - while most have only one persona, but a company can have multiple: it is a startup and also an social enterprise, or a government agency who is also an ecosystem builder
  * [Industry](Master-Data-Industry) - which industry is this company involved in, most companies are across multiple industries
  * [SDG](Master-Data-SDG) - which SDG this company try to solve, multiple allowed
  * [Startup Stages](Master-Data-Startup-Stages) - which stages this company is in if it is a startup, only one stage at a time.
  * [Legal Form](Master-Data-Legal-Form)

Organization title is unique throughout the system, which means once taken, it is no longer available for others.

## Individual

Individuals are person, they can founders of tech startups, or student aspiring to be an entrepreneurs. 

![Untitled drawing](https://user-images.githubusercontent.com/5336690/83710843-47c86300-a654-11ea-93c8-cc9747ffea43.png)
Individual is links to user thru verified emails. Do not confuse between user, profile, member and individual. Use the diagram above to understand their relationship.

  * [Persona](Master-Data-Persona)

## Event
Most of the activities happened in startup ecosystem can be break down and categorized as Event. Tracking event allows ecosystem builder to understand the development of an ecosystem.

  * [Persona](Master-Data-Persona) - this event is targeting for attendees of what kind of persona
  * [Industry](Master-Data-Industry) - this event is for attendees of which industry
  * [Startup Stages](Master-Data-Startup-Stages) - this event is targeting for startup at which stages

### Event Group
Multiple events can be group under 1 Event Group. For example, a 'MaGIC Accelerator Program' is an event group with the following events:
  * MaGIC Accelerator Program 2015
  * MaGIC Accelerator Program 2016
  * MaGIC Accelerator Program 2017

### Event Registration
Event registration keep track of individuals who participated in an event. 

These data can be sync thru module like `Eventbrite` and `Bizzabo`. It can also be manually bulk loaded using spreadsheet. (todo: attach link of the spreadsheet template here).

### Event Organization
Event Organization keep track of companies/organizations who participated in an event. For examples, an event would be organised by `MaGIC` and `Ministry of Technology`, sponsored by `Google` and `Facebook`, with judges came from `Multimedia University` and `Startup Melaka`.

### Survey
When survey is enabled and automatically sent to attended participants a day after the event ended. For this to happens:
* Event has to be active and not cancel
* Setting `event-sendPostSurveyEmail` must be turned on
* Hourly cron job is set to trigger the command `php yiic emailSurveyAfterEvent oneDayAfter`
* Respective F7 form must exists

#### Todo
Survey is hard corded by @Mahboubian and this need to be improve to a modularise and flexible manners.

```php
public function getSurveyTypes($eventId)
{
    return array(
        '1Day' => array('numberOfDays' => '1', 'formSlug' => 'Feedback1', 'participantsMode' => 'attended'),
        '6Months' => array('numberOfDays' => '180', 'formSlug' => 'Feedback2', 'participantsMode' => 'attended'),
    );
}
```