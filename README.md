# Duo REST Collector IO
----
## About this Pack

This pack is built as a complete SOURCE + DESTINATION solution (identified by the IO suffix). Data collection and delivery happen entirely within the pack's context, eliminating the need to connect it to globally defined Sources and Destinations. 

This Pack is designed to collect, process, and output Duo log data via the [Duo Admin REST API](https://duo.com/docs/adminapi).

The Pack includes (optional) Splunk output processing that maps data to the Splunk source=`sbg_duo_input://cribl_duo_api` for compatibility with the [Cisco Security Cloud App](https://splunkbase.splunk.com/app/7404). (The Duo Splunk Connector App was deprecated on 1-Sept-2025)

## Deployment

* This pack is configured by default to use the Worker Group's *Default Destination*.
* To use the *Default Destination*: No changes are required. The pack will route the data to the destination currently set as the Default on the Worker Group.
* To use a different Destination: You must update the pack's routes to specify your desired Destination.
* For immediate functionality without requiring Pack route filter expression modifications, every bundled Source within this pack adds a hidden field: `__packsource`. This field allows for seamless routing based on the Pack source.

### Configure HMAC Secrets

*Note*: If you use an external secret store, the HMAC function will have to be modified to reference those secrets using the proper names.

* Obtain an **Secret key**, **Integration key**, and **API hostname** from your Duo Admin API application.
* Open the worker group that will contain the Duo collector.
* Navigate to **Worker Group Settings > Security > Secrets**
* Create 2 Secrets. 
   * The first will be `duo-admin-secret-key`, of secret type **text** and will conain the **Secret key**. 
   * The second will be `duo-admin-integration-key`, of secret type **text** and will conain the **Integration key**. 

### Configure the Collectors

* Navigate to the ** sources > collectors > rest ** with the Duo pack. There are 6 collectors availavble.
   * `in_duo_authentication`: This API collects [Authentication](https://duo.com/docs/adminapi#authentication-logs) logs and is currently at V2.
   * `in_duo_activity`: This API collects [Activity](https://duo.com/docs/adminapi#activity-logs) logs and is currently at V2.
   * `in_duo_administrator`: This API collects [Administrator](https://duo.com/docs/adminapi#administrator-logs) logs and is currently at V1.
   * `in_duo_telephony`: This API collects [Telephony](https://duo.com/docs/adminapi#telephony-logs) logs and is currently at V2.
   * `in_duo_offline_enrollment`: This API collects [Offline Enrollment](https://duo.com/docs/adminapi#offline-enrollment-logs) logs and is currently at V1.
   * `in_duo_users`: This API collects [Users](https://duo.com/docs/adminapi#users) data and is currently at V1. This API pulls in user inventory, rather than log data.

* Replace the `API-HOSTNAME` in the **Collect URL** parameter of each collector with the **API hostname** value provided in the Duo Admin API portal.
* Perform a Run > Preview to verify that each Collector works correctly.
* Schedule data ingestion by clicking the **Schedule** button under the Actions column of the Collector and toggle the **Enabled** button to enable data flow. Modify the cron schedule if required:
  * The default for log data is to run every 5 minutes. 
  * The default for the users inventory data is to run once, nightly.

### Configure your Destination/Update Pack Routes
To ensure proper data routing, you must make a choice: retain the current setting to use the Default Destination defined by your Worker Group, or define a new Destination directly inside this pack and adjust the pack's route accordingly.

This Pack includes a Splunk HEC Output. To configure it:

* Create a [HEC token in Splunk](https://help.splunk.com/en/splunk-enterprise/get-started/get-data-in/10.0/get-data-with-http-event-collector/set-up-and-use-http-event-collector-in-splunk-web).
* Populate the **Splunk HEC Endpoints** parameter with the Splunk HEC URL.
* Populate the **HEC Auth token** with the token created in Splunk

### Commit and Deploy
Once everything is configured, perform a Commit & Deploy to enable data collection.

## Pack Configurable Items 
The following are the in-Pack configurable items - review/update them as needed. 

### Variables

The Pack has the following variables:
* `default_splunk_index`: Default index for the Splunk output - defaults to `duo`.
* `default_splunk_source`: Default source for the Splunk output - defaults to `sbg_duo_input://cribl_duo_api`.
* `default_splunk_sourcetype`: Default sourcetype for the Splunk output - defaults to `duo:api`.

## Upgrades

Upgrading certain Cribl Packs using the same Pack ID can have unintended consequences. See [Upgrading an Existing Pack](https://docs.cribl.io/stream/packs#upgrading) for details.

## Release Notes

### Version 1.0.0
Initial release

## Contributing to the Pack

To contribute to the Pack, please connect with us on [Cribl Community Slack](https://cribl-community.slack.com/). You can suggest new features or offer to collaborate.

## License
This Pack uses the following license: [Apache 2.0](https://github.com/criblio/appscope/blob/master/LICENSE).