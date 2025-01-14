# Greengrass nucleus<a name="greengrass-nucleus-component"></a>

The Greengrass nucleus component \(`aws.greengrass.Nucleus`\) is the only mandatory component and the minimum requirement to run the AWS IoT Greengrass Core software on a device\. You can configure this component to customize and update your AWS IoT Greengrass Core software remotely\. You can deploy this component to configure settings such as proxy, device role, and AWS IoT thing configuration on your core devices\.

**Important**  
When you change the version of this component, or when you change certain parameters, the AWS IoT Greengrass Core software and all components restart to apply the changes\.

For more information about requirements and how to configure this component, see [Configure the AWS IoT Greengrass Core software](configure-greengrass-core-v2.md) and [Update the AWS IoT Greengrass Core software \(OTA\)](update-greengrass-core-v2.md)\.

This component has the following versions:
+ 2\.0\.x

## Configuration<a name="greengrass-nucleus-component-configuration"></a>

This component provides the following configuration parameters that you can customize when you deploy the component\. Some parameters require that the AWS IoT Greengrass Core software restarts to take effect\.

`iotRoleAlias`  
The AWS IoT role alias that points to an IAM role\. The AWS IoT credentials provider assumes this role to allow the Greengrass core device to interact with AWS services\.  
When you run the AWS IoT Greengrass Core software with the `--provision true` option, the software provisions a role alias and sets its value in the nucleus component\. For more information, see [Authorize core devices to interact with AWS services](device-service-role.md)\.

`networkProxy`  
\(Optional\) The network proxy to use for all connections\. For more information, see [Connect on port 443 or through a network proxy](configure-greengrass-core-v2.md#configure-alpn-network-proxy)\.  
When you deploy a change to this configuration parameter, the AWS IoT Greengrass Core software restarts for the change to take effect\.
This object contains the following information:    
`noProxyAddresses`  
\(Optional\) A comma\-separated list of IP addresses or host names that are exempt from the proxy\.  
`proxy`  
The proxy to which to connect\. This object contains the following information:    
`url`  
The URL of the proxy server in the format `scheme://userinfo@host:port`\.  
+ `scheme` – The scheme, which must be `http` or `https`\.
+ `userinfo` – \(Optional\) The user name and password information\. If you specify this in the `url`, the Greengrass core device ignores the `username` and `password` fields\.
+ `host` – The host name or IP address of the proxy server\.
+ `port` – \(Optional\) The port number\. If you don't specify the port, then the Greengrass core device uses the following default values:
  + `http` – 80
  + `https` – 443  
`username`  
\(Optional\) The user name to use to authenticate to the proxy server\.  
`password`  
\(Optional\) The password to use to authenticate to the proxy server\.

`mqtt`  
\(Optional\) The MQTT configuration for the Greengrass core device\. For more information, see [Connect on port 443 or through a network proxy](configure-greengrass-core-v2.md#configure-alpn-network-proxy)\.  
When you deploy a change to this configuration parameter, the AWS IoT Greengrass Core software restarts for the change to take effect\.
This object contains the following information:    
`port`  
\(Optional\) The port to use for MQTT connections\.  
Default: `8883`  
`keepAliveTimeoutMs`  
\(Optional\) The amount of time in milliseconds between each `PING` message that the client sends to keep the MQTT connection alive\.  
Default: `60000` \(60 seconds\)  
`pingTimeoutMs`  
\(Optional\) The amount of time in milliseconds that the client waits to receive a `PINGACK` message from the server\. If the wait exceeds the timeout, the core device closes and reopens the MQTT connection\.  
Default: `30000` \(30 seconds\)  
`spooler`  
\(Optional\) The MQTT spooler configuration for the Greengrass core device\. This object contains the following information:    
`maxSizeInBytes`  
\(Optional\) The maximum size of the cache where the core device stores unprocessed MQTT messages in memory\. If the cache is full, the core device discards the oldest messages to add new mesasges\.  
Default: `2621440` \(2\.5 MB\)  
`keepQos0WhenOffline`  
\(Optional\) You can spool MQTT QoS 0 messages that the core device receives while its offline\. If you set this option to `true`, the core device spools QoS 0 messages that it can't send while it's offline\. If you set this option to `false`, the core device discards these messages\. The core device always spools QoS 1 messages unless the spool is full\.  
Default: `false`

`jvmOptions`  
\(Optional\) The JVM options to use to run the AWS IoT Greengrass Core software\.  
When you deploy a change to this configuration parameter, the AWS IoT Greengrass Core software restarts for the change to take effect\.

`iotDataEndpoint`  
The AWS IoT data endpoint for your AWS account\.  
<a name="nucleus-component-set-iot-endpoints"></a>When you run the AWS IoT Greengrass Core software with the `--provision true` option, the software gets your data and credentials endpoints from AWS IoT and sets them in the nucleus component\.

`iotCredEndpoint`  
The AWS IoT credentials endpoint for your AWS account\.  
<a name="nucleus-component-set-iot-endpoints"></a>When you run the AWS IoT Greengrass Core software with the `--provision true` option, the software gets your data and credentials endpoints from AWS IoT and sets them in the nucleus component\.

`awsRegion`  
The AWS Region to use\.

`runWithDefault`  
The system user and group to use to run components\.  
When you deploy a change to this configuration parameter, the AWS IoT Greengrass Core software restarts for the change to take effect\.
This object contains the following information:    
`posixUser`  
The name or ID of the system user and system group that the core device uses to run components\. Specify the user and group separated by a colon \(`:`\), where the group is optional\. If you omit the group, the AWS IoT Greengrass Core software defaults to the primary group of the user that you specify\. For example, you can specify `ggc_user` or `ggc_user:ggc_group`\. For more information, see [Configure the user and group that run components](configure-greengrass-core-v2.md#configure-component-user)\.  
When you run the AWS IoT Greengrass Core software with the `--component-default-user ggc_user:ggc_group` option, the software sets this parameter in the nucleus component\.  
You can't specify `root` or `0` for the user or the group\.

`logging`  
\(Optional\) The logging configuration for the core device\. This object contains the following information:    
`level`  
\(Optional\) The minimum level of information to log\. Choose from the following log levels, listed here in level order:  
+ `DEBUG`
+ `INFO`
+ `WARN`
+ `ERROR`
Default: `INFO`  
`format`  
\(Optional\) The data format of the logs\. Choose from the following options:  
+ `TEXT`
+ `JSON`
Default: `TEXT`  
`outputType`  
\(Optional\) The output type for logs\. Choose from the following options:  
+ `FILE` – The AWS IoT Greengrass Core software outputs logs to files in the directory that you specify in `outputDirectory`\.
+ `CONSOLE` – The AWS IoT Greengrass Core software prints logs to `stdout`\. Choose this option to view logs as the core device prints them\.
Default: `FILE`  
`fileSizeKB`  
\(Optional\) The maximum size of each log file \(in kilobytes\)\. After a log file reaches exceeds this maximum file size, the AWS IoT Greengrass Core software creates a new log file\.  
<a name="nucleus-component-logging-parameter-file-only"></a>This parameter applies only when you specify `FILE` for `outputType`\.  
Default: `1024`  
  `totalLogsSizeKB`   
\(Optional\) The maximum total size of all log files \(in kilobytes\)\. After the total size of all log files exceeds this maximum total size, the AWS IoT Greengrass Core software deletes the oldest log files\.  
This parameter is equivalent to the [system logs disk space limit](log-manager-component.md#log-manager-component-configuration-system-logs-limit) parameter of the [log manager component](log-manager-component.md)\. If you specify this parameter on both components, the AWS IoT Greengrass Core software uses the minimum of the two values as the maximum total log size\.  
<a name="nucleus-component-logging-parameter-file-only"></a>This parameter applies only when you specify `FILE` for `outputType`\.  
Default: `10240`  
`outputDirectory`  
\(Optional\) The output directory for log files\.  
<a name="nucleus-component-logging-parameter-file-only"></a>This parameter applies only when you specify `FILE` for `outputType`\.  
Default: `/greengrass/v2/logs`, where */greengrass/v2* is the AWS IoT Greengrass root folder\.

  `telemetry`   
\(Optional\) The system health telemetry configuration for the core device\. For more information about telemetry metrics and how to act on telemetry data, see [Gather system health telemetry data from AWS IoT Greengrass core devices](telemetry.md)\.  
This object contains the following information:    
`enabled`  
\(Optional\) You can enable telemetry\.  
Default: `true`  
`periodicAggregateMetricsIntervalSeconds`  
\(Optional\) The interval \(in seconds\) over which the core device aggregates metrics\.  
Minimum: `3600`  
Default: `3600`  
`periodicPublishMetricsIntervalSeconds`  
\(Optional\) The amount of time \(in seconds\) between which the core device publishes telemetry metrics to the AWS Cloud\.  
Minimum: `86400`  
Default: `86400`

`deploymentPollingFrequency`  
\(Optional\) The period in seconds at which to poll for deployment notifications\.  
Default: `15`

`componentStoreMaxSize`  
\(Optional\) The maximum size on disk of the component store, which comprises component recipes and artifacts\.  
Default: `10000000000` \(10 GB\)

  `platformOverrides`   
\(Optional\) A dictionary of attributes that identify the core device's platform\. Use this to define custom platform attributes that component recipes can use to identify the correct lifecycle and artifacts for the component\. For example, you might define a hardware capability attribute to deploy only the minimal set of artifacts for a component to run\. For more information, see the [manifest platform parameter](component-recipe-reference.md#component-platform-definition) in the component recipe\.  
You can also use this parameter to override the `os` and `architecture` platform attributes of the core device\.