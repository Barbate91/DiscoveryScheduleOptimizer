# DiscoveryScheduleOptimizer
ServiceNow Script Include

  
/*
 * DiscoveryScheduleOptimizer
 *
 * Get the monthly average duration for all Discovery Schedules in our GlideRecord query and sort by longest average duration.
 * Then space the run times for each Discovery Schedule according to an "off peak" start time in order to minimize Discovery load on the
 * instance and stagger based on average run times.
 *
 * If desired, this will also set the max run time equal to the average duration. The idea being that once a client's schedules are spaced out
 * that their average run duration will go down, meaning that the schedules shouldn't take as long to complete. 
 *
 * This is also an iterative process, where the optimizer should be run again at a later date to account for the now lower average run duration 
 * and to reorganize the schedules accordingly.
 *
 * Note that this will loop schedule run times and may overlap depending on the avg durations. For example, if you have 36 daily schedules
 * that avg 1 hour each, it will not be 24/36 for the run times. It would be 1 schedule for each hour until 24 hours, and then another schedule 
 * for each hour (24 schedules between 18:00-06:00, 12 schedules between 06:00-18:00).
 *
 * Example input parameters:
 * var timezone = "US/Pacific"; -- A valid Java timezone ID
 * var offPeakHour = "1970-01-01 18:00"; -- YYYY-MM-DD doesn't matter, but we need this format for conversions
 * var encodedQuery = "disco_run_type!=on_demand^ORdisco_run_type=NULL"; -- Unsupported schedule types are gracefully ignored
 * var maxRun = true; -- False doesn't reset max run time and will ignore existing run times
 *
 * TODO: Evaluate supporting other schedule types (daily/weekly/monthly currently supported)
 * TODO: Add additional error handling and notify user in form instead of syslog
 *
 * @author jake.armstrong
 */
