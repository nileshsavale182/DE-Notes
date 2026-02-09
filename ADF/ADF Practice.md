
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
* A tumbling window trigger divides time into fixed, consecutive intervals, while retaining the state.
   * ✔ It ensures the next window does not start until the previous window’s state is successful (When concurrency = 1)
   * Max Concurrency setting helps running multiple windows simultaneously. 
* Each interval is processed exactly once unless manually re‑executed.
* Windows never overlap; end of one is the start of the next.
* It is designed for deterministic, repeatable time‑based data processing, it is strictly time-driven.

##### Trigger Inputs & System Variables
* Pipelines receive system variables windowStart and windowEnd for time‑slice logic.
   * trigger().output().windowStartTime
   * trigger().output().windowEndTime
* Simplifies incremental queries by providing precise timestamps for filtering.

##### Execution Behavior
* Execution depends on successful completion of earlier windows by default.
* Failed windows block subsequent windows unless retried or run manually.
* Best suited for batch scenarios requiring strict time ordering.

##### Backfill & Reprocessing
* It supports automatic backfill of missed windows when the trigger becomes active again.
* Allows reprocessing of any past window without affecting future windows.

##### Configuration Options
* The trigger can be configured with frequency and interval (e.g., every hour/day).
* A delay can be added to allow late-arriving data before processing begins.
* Max concurrency controls how many windows can run simultaneously.
* Supports retry policies to handle transient errors during execution.

##### Best Use Cases
* Useful for incremental data loads where each batch corresponds to a specific time range.
   * If ADF was paused, it backfills missed windows
     * This allows incremental loads to remain consistent during downtimes.
     * Example: ADF was paused for 6 hours → 6 windows are automatically queued when enabled.
* Ideal for processing partitioned datasets stored by date/time structure.
* Ensures predictable, consistent execution for time-bound ETL pipelines.
* More stateful compared to schedule triggers because it tracks window progress.
<hr> 

##### Tumbling Window Interview Questions 
🔶 1. What is a tumbling window trigger and when would you use it over a schedule trigger?
Focus on explaining stateful, non-overlapping, backfill-enabled behavior.

🔶 2. How do windowStart and windowEnd support incremental data loading?
Explain time-slice filtering like:
SQLWHERE UpdatedOn >= windowStart AND UpdatedOn < windowEndShow more lines

🔶 3. What happens if a tumbling window fails? Will the next window run?
Expected:
No. The next window waits until previous one succeeds.

🔶 4. How does backfill work in tumbling window triggers?
Explain how missed windows are generated and executed automatically after enabling the trigger.

🔶 5. What is Max Concurrency in tumbling window triggers? When would you increase/decrease it?
Give examples:

Increase for catching up backlog
Decrease for strict sequential pipelines


🔶 6. What happens if upstream dependency didn’t trigger but the window arrived?
Expected:

Downstream window goes into "Waiting on dependency"
It waits up to 7 days (default, non-configurable)


🔶 7. Is the 7‑day dependency wait configurable?
Correct answer:
No, it is fixed by ADF and cannot be reduced.
Retry policy does not modify this.

🔶 8. If dependency is already completed but the window didn’t trigger, what can you do?
Correct answer:

Manually Rerun the specific window
ADF allows forcing execution of individual windows


🔶 9. Explain dependency offset and dependency size with an example.
Examples:

Offset = -1 hr means current window depends on previous window of upstream trigger
Size = 2 hrs means it depends on two consecutive upstream windows


🔶 10. How do you handle late-arriving data in a tumbling window pipeline?
Expected:

Use Delay property (e.g., 10 minutes)
Ensures late files arrive before the window runs


🔶 11. How do you reprocess only one specific window if data was incorrect?
Use Manual Rerun for that window.

🔶 12. If your trigger was disabled for 3 days, what happens when you enable it?
Expected:

ADF generates 3 days worth of windows
Executes them sequentially or in parallel depending on Max Concurrency


🔶 13. What happens if a window is running longer than the next window time?
Expected:

Tumbling windows never overlap
Next window waits until current window completes


🔶 14. What conditions cause a window to go into “Waiting” state?
Possible reasons:

Upstream dependency incomplete
Backfill windows pending
Previous window still running
Trigger disabled during the window time


🔶 15. How do you debug a tumbling window trigger that is not firing?
Typical steps:

Check dependency trigger status
Verify previous window success
Inspect backfill queue
Check end time/start time configuration
Make sure the trigger is actually started


🔶 16. Can you have multiple tumbling window triggers on the same pipeline? Why?
Yes — to support different time slices or independent orchestration paths.

🔶 17. Difference between self-dependency and dependency on another trigger?

Self-dependency: Same trigger depends on earlier windows
Trigger dependency: Depends on a different tumbling window trigger


🔶 18. How do you avoid double‑processing data when using tumbling windows?
Expected:

Use strict >= windowStart AND < windowEnd logic
Windows are non-overlapping


🔶 19. What happens when end time of tumbling window trigger is reached?
ADF stops generating new windows automatically.

🔶 20. Why tumbling windows are called “stateful”?
Expected:

Tracks each window’s success/failure
Controls execution order
Supports backfill
Maintains dependency state











