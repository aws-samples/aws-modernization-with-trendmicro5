---
title: "Security Tools - DAST"
chapter: true
weight: 53
pre: "<b>5.3. </b>"
---

# Security Tools

---

## Dynamic application security testing – DAST

For this part of the workshop we will use ***Zaproxy*** tool as a DAST tool to test the security of our application. 

From the [Zaproxy Github page](https://github.com/zaproxy/zaproxy):  

> The OWASP Zed Attack Proxy (ZAP) is one of the world’s most popular free security tools and is actively maintained by a dedicated international team of volunteers. It can help you automatically find security vulnerabilities in your web applications while you are developing and testing your applications. It's also a great tool for experienced pentesters to use for manual security testing. 

---

### 1. In the terminal create the configuration file so that we can use Zaproxy 

- Type the command ```cd .. ``` 
- Then type the command ```touch zap-framework.yaml```
- To open the file in VS Code, type the command ```code zap-framework.yaml``` 

**Paste the following:**
     
    # OWASP ZAP automation configuration file.
    env:                                      # The environment, mandatory
      contexts:                               
        - name: pygoat-app                    # Name to be used to refer to this context in other jobs, mandatory
          urls: [your_url_with_port_here]     # The top level urls, mandatory, everything under this will be included
          includePaths:  
          excludePaths:  
      parameters:
        failOnError: true                  # If set exit on an error
        failOnWarning: false               # If set exit on a warning
        progressToStdout: true             # If set will write job progress to stdout
    
    jobs:
      - type: addOns                       # Add-on management
        parameters:
          updateAddOns: false               # Update any add-ons that have new versions
        install:                            # A list of non standard add-ons to install from the ZAP Marketplace
          - reports
      - type: passiveScan-config           # Passive scan configuration
        parameters:
          maxAlertsPerRule: 10             # Int: Maximum number of alerts to raise per rule
          scanOnlyInScope: true            # Bool: Only scan URLs in scope (recommended)
          maxBodySizeInBytesToScan:        # Int: Maximum body size to scan, default: 0 - will scan all messages
        rules:                             # A list of one or more passive scan rules and associated settings which override the defaults
      
      - type: spider                       # The traditional spider - fast but doesnt handle modern apps so well
        parameters:
          context:                         # String: Name of the context to spider, default: first context
          url:                             # String: Url to start spidering from, default: first context URL
          failIfFoundUrlsLessThan:         # Int: Fail if spider finds less than the specified number of URLs, default: 0
          warnIfFoundUrlsLessThan:         # Int: Warn if spider finds less than the specified number of URLs, default: 0
          maxDuration:                     # Int: The max time in minutes the spider will be allowed to run for, default: 0 unlimited
          maxDepth:                        # Int: The maximum tree depth to explore, default 5
          maxChildren:                     # Int: The maximum number of children to add to each node in the tree
          acceptCookies:                   # Bool: Whether the spider will accept cookies, default: true
          handleODataParametersVisited:    # Bool: Whether the spider will handle OData responses, default: false
          handleParameters:                # Enum [ignore_completely, ignore_value, use_all]: How query string parameters are used when checking if a URI has already been visited, default: use_all
          maxParseSizeBytes:               # Int: The max size of a response that will be parsed, default: 2621440 - 2.5 Mb
          parseComments:                   # Bool: Whether the spider will parse HTML comments in order to find URLs, default: true
          parseGit:                        # Bool: Whether the spider will parse Git metadata in order to find URLs, default: false
          parseRobotsTxt:                  # Bool: Whether the spider will parse 'robots.txt' files in order to find URLs, default: true
          parseSitemapXml:                 # Bool: Whether the spider will parse 'sitemap.xml' files in order to find URLs, default: true
          parseSVNEntries:                 # Bool: Whether the spider will parse SVN metadata in order to find URLs, default: false
          postForm:                        # Bool: Whether the spider will submit POST forms, default: true
          processForm:                     # Bool: Whether the spider will process forms, default: true
          requestWaitTime:                 # Int: The time between the requests sent to a server in milliseconds, default: 200
          sendRefererHeader:               # Bool: Whether the spider will send the referer header, default: true
          threadCount:                     
          userAgent: ''                    # String: The user agent to use in requests, default: '' - use the default ZAP one
      - type: passiveScan-wait             # Passive scan wait for the passive scanner to finish
        parameters:
          maxDuration:                     # Int: The max time to wait for the passive scanner, default: 0 unlimited
      - type: activeScan                   # The active scanner - this actively attacks the target so should only be used with permission
        parameters:
          context:                         # String: Name of the context to attack, default: first context
          policy:                          # String: Name of the scan policy to be used, default: Default Policy
          maxRuleDurationInMins:           # Int: The max time in minutes any individual rule will be allowed to run for, default: 0 unlimited
          maxScanDurationInMins: 7         # Int: The max time in minutes the active scanner will be allowed to run for, default: 0 unlimited
          addQueryParam:                   # Bool: If set will add an extra query parameter to requests that do not have one, default: false
          defaultPolicy:                   # String: The name of the default scan policy to use, default: Default Policy
          delayInMs:                       # Int: The delay in milliseconds between each request, use to reduce the strain on the target, default 0
          handleAntiCSRFTokens:            # Bool: If set then automatically handle anti CSRF tokens, default: false
          injectPluginIdInHeader:          # Bool: If set then the relevant rule Id will be injected into the X-ZAP-Scan-ID header of each request, default: false
          scanHeadersAllRequests:          # Bool: If set then the headers of requests that do not include any parameters will be scanned, default: false
          threadPerHost:                   
        policyDefinition:                  # The policy definition - only used if the 'policy' is not set
          defaultStrength:                 # String: The default Attack Strength for all rules, one of Low, Medium, High, Insane (not recommended), default: Medium
          defaultThreshold:                # String: The default Alert Threshold for all rules, one of Off, Low, Medium, High, default: Medium
      - type: report                       # Report generation
        parameters:
          template: traditional-html       # String: The template id, default : traditional-html
          reportDir: /zap/wrk/             # String: The directory into which the report will be written
          reportFile:                      # String: The report file name pattern, default: {yyyy-MM-dd}-ZAP-Report-[[site]]
          reportTitle: Pygoat-Zaproxy-Results         # String: The report title
          reportDescription:               # String: The report description
          displayReport: false             # Boolean: Display the report when generated, default: false
        risks:                             # List: The risks to include in this report, default all

        confidences:                       # List: The confidences to include in this report, default all

        sections:                          # List: The template sections to include in this report - see the relevant template, default all

      - type: report                       # Report generation
        parameters:
          template: traditional-json       # String: The template id, default : traditional-html
          reportDir: /zap/wrk/             # String: The directory into which the report will be written
          reportFile:                      # String: The report file name pattern, default: {yyyy-MM-dd}-ZAP-Report-[[site]]
          reportTitle: Pygoat-Zaproxy-Results         # String: The report title
          reportDescription:               # String: The report description
          displayReport: false             # Boolean: Display the report when generated, default: false
        risks:                             # List: The risks to include in this report, default all

        confidences:                       # List: The confidences to include in this report, default all

        sections:                          # List: The template sections to include in this report - see the relevant template, default all

![DAST](/images/dast1.PNG)

{{% notice warning %}}
<p style='text-align: left;'>
Change in URLS <i><b>your_url_with_port_here</b></i> to your application's URL.
</p>
{{% /notice %}}

![DAST](/images/dast2.PNG)

---

### 2. Run the docker container to attack the application using the configuration provided before

    docker run -v $(pwd):/zap/wrk/:rw -t owasp/zap2docker-stable bash -c "zap.sh -cmd -autorun /zap/wrk/zap-framework.yaml"

![DAST](/images/dast3.PNG)

![DAST](/images/dast4.PNG)

![DAST](/images/dast5.PNG)

{{% notice info %}}
<p style='text-align: left;'>
It should take more or less 7 min to finish the attack using the configurations we set
</p>
{{% /notice %}}

---

### 3. In AWS Console

- Go to **CloudWatch** > **Log groups**
- Search for the logs created by your **NSasS** deployment

![DAST](/images/c1ns51.PNG)

- At the bottom of this screen in **Log Streams**
- Click on **ipsBlock**

![DAST](/images/c1ns52.PNG)


- You should start to see some block actions being done by Network Security

![DAST](/images/dast6.PNG)

---

### If you want you can also see Zaproxy's own report

- In the same directory that you ran the tool, you will have two types of reports in two different formats, one in **JSON** and other in **HTML**
- To see them, type the command ```ls    ```
- And the command ```code  ``` and the name of the report you want to see

![DAST](/images/dast7.PNG)

--------

### Congrats on exploring vulnerabilities and attacking your web application! With the tools deployed before now you know that your it is more secure! :milky_way: :face_in_clouds: