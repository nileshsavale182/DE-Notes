
# <font color="red">ADF Triggers</font>

* Triggers - you can execute your pipeline
* Triggers determine when a pipleline execution needs to be kicked off
* Pipeline and triggers have many to many relationship (except for tumbling window trigger)
* Multiple triggers can kick off single pipeline or a single trigger can kick off multiple pipelines

### Type of Triggers 
* **Scheduled trigger** - a trigger that invokes a pipeline on a wall clock schedule
* **Tumbling window trigger** - A trigger that operates on a periodic interval while also retaining the state.
* **Event Based Trigger** - A trigger that responds to the trigger

<hr>

#### Scheduled Trigger

* Scheduled trigger runs pipeline on wall clock schedule
* This trigger supports periodic and advanced calender options.
* For e.g. the trigger supports intervals like 'weekly' or 'monday at 5:00 PM
* Name > Type > Start Date > TimeZone > Recurrence (every x minutes, hours, days, weeks, months) > specify end date (checkbox) > start trigger on creation (checkbox) i.e. activated.
* If we do not specify end date, it runs forever
<img width="30%" height="30%" alt="image" src="https://github.com/user-attachments/assets/a6922ea8-17aa-474a-a764-4b7a61238c11" />

<hr>

#### Tumbling window trigger
* A tumbling window trigger divides time into fixed, consecutive intervals.
*  Each interval is processed exactly once unless manually re‑executed.
*  Windows never overlap; end of one is the start of the next.
*  Pipelines receive system variables windowStart and windowEnd for time‑slice logic.
  * 

















