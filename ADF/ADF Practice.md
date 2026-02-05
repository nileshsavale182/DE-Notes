
# <font color="red">ADF Triggers</font>

* Triggers - you can execute your pipeline
* Triggers determine when a pipleline execution needs to be kicked off
* Pipeline and triggers have many to many relationship (except for tumbling window trigger)
* Multiple triggers can kick off single pipeline or a single trigger can kick off multiple pipelines

### Type of Triggers 
* **Scheduled trigger** - a trigger that invokes a pipeline on a wall clock schedule
* **Tumbling window trigger** - A trigger that operates on a periodic interval while also retaining the state.
* **Event Based Trigger** - A trigger that responds to the trigger

#### Scheduled Trigger
* Scheduled trigger runs pipeline on wall clock schedule
* This trigger supports periodic and advanced calender options.
* For e.g. the trigger supports intervals like 'weekly' or 'monday at 5:00 PM
* Name > Type > Start Date > TimeZone > Recurrence (every x minutes, hours, days, weeks, months) > specify 
