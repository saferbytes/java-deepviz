# JDeepviz
JDeepviz is a Java wrapper for deepviz.com REST APIs

# Install

JDeepviz is hosted by Maven central repository. Add the following dependency in your pom.xml file.

```xml
<dependencies>
    <dependency>
        <groupId>com.mashape.unirest</groupId>
        <artifactId>unirest-java</artifactId>
        <version>1.4.7</version>
    </dependency>
</dependencies>
```

# Usage
To use Deepviz API sdk you will need an API key you can get by
subscribing the service free at https://account.deepviz.com/register/

# Sandbox SDK API

To upload a sample:

```java
import deepviz.Result;
import deepviz.sandbox.Sandbox;

Sandbox sbx = new Sandbox();
Result result = sbx.uploadSample("my-api-key","path\\to\\file.exe")
System.out.println(result);
```

To upload a folder:

```java
import deepviz.sandbox.Sandbox;

Sandbox sbx = new Sandbox();
sbx.uploadFolder("my-api-key","path\\to\\file.exe")
```

To download a sample:

```java
import deepviz.Result;
import deepviz.sandbox.Sandbox;

Sandbox sbx = new Sandbox();
Result result = sbx.downloadSample("my-api-key", "md5-to-download", "dest-path")
System.out.println(result);
```

To send a bulk download request:

```java
import java.util.ArrayList;
import java.util.List;

import deepviz.sandbox.Sandbox;
import deepviz.Result;

List<String> md5_list = new ArrayList<String>();
md5_list.add("a6ca3b8c79e1b7e2a6ef046b0702aeb2");
md5_list.add("34781d4f8654f9547cc205061221aea5");
md5_list.add("a8c5c0d39753c97e1ffdfc6b17423dd6");

Result result =  sbx.bulkDownloadRequest("my-api-key", md5_list)
System.out.println(result);
```

To download the archive af a bulk download request:

```java
import deepviz.Result;
import deepviz.sandbox.Sandbox;

Sandbox sbx = new Sandbox();
Result result = sbx.bulkDownloadRetrieve("my-api-key", "id-request", "dest-path");
System.out.println(result);
```

To retrieve scan result of a specific MD5

```java
import deepviz.Result;
import deepviz.sandbox.Sandbox;

Result result = sbx.sampleResult("my-api-key", "MD5-hash");
System.out.print(result.getMsg());
```

To retrieve full scan report for a specific MD5

```java
import java.util.ArrayList;
import java.util.List;

import deepviz.sandbox.Sandbox;
import deepviz.Result;

Result result = sbx.sampleReport("my-api-key", "MD5-hash");
System.out.print(result.getMsg());
```

To retrieve only specific parts of the report of a specific MD5 scan

```java
import deepviz.sandbox.Sandbox;
import deepviz.Result;

import java.util.ArrayList;
import java.util.List;

List<String> filters = new ArrayList<String>();
filters.add("classification");
filters.add("rules");
Result result = sbx.sampleReport("my-api-key", "MD5-hash", filters);

# List of the optional filters - they can be combined together
# "network_ip",
# "network_ip_tcp",
# "network_ip_udp",
# "rules",
# "classification",
# "created_process",
# "hook_user_mode",
# "strings",
# "created_files",
# "hash",
# "info",
# "code_injection"

System.out.println(result.getMsg());
```

# Threat Intelligence SDK API

To retrieve intel data about one or more IPs:

```java
import deepviz.intel.input.IpInfoInput;
import deepviz.intel.Intel;
import deepviz.Result;

import java.util.ArrayList;
import java.util.List;

List<String> ip_list = new ArrayList<String>();
ip_list.add("8.8.8.8");

IpInfoInput input = new IpInfoInput();
input.setIps(ip_list);
input.setHistory(true);

result = intel.ipInfo("my-api-key", input);
System.out.println(result);
```

To retrieve intel data about IPs contacted in the last 7 days:

```java
import deepviz.intel.input.IpInfoInput;
import deepviz.intel.Intel;
import deepviz.Result;

IpInfoInput input = new IpInfoInput();
input.setTimeDelta("7d");
result = intel.ipInfo("my-api-key", input);
System.out.println(result);
```

To retrieve intel data about one or more domains:

```java
import deepviz.intel.input.DomainInfoInput;
import deepviz.intel.Intel;
import deepviz.Result;

import java.util.ArrayList;
import java.util.List;

List<String> filters = new ArrayList<String>();
filters.add("sub_domains");
# List of the optional filters - they can be combined together
# "whois",
# "sub_domains"

List<String> domains = new ArrayList<String>();
domains.add("google.com");

DomainInfoInput input = new DomainInfoInput();
input.setDomains(domains);
input.setFilters(filters);

Result result = intel.domainInfo("my-api-key", input);
System.out.println(result);
```

To retrieve newly registered domains in the last 7 days:

```java
import deepviz.intel.input.DomainInfoInput;
import deepviz.intel.Intel;
import deepviz.Result;

DomainInfoInput input = new DomainInfoInput();
input.setTimeDelta("7d");
Result result = intel.domainInfo("my-api-key", input);
System.out.println(result);
```

To run generic search based on strings
(find all IPs, domains, samples related to the searched keyword):

```java
import deepviz.Result;
import deepviz.intel.Intel;

Intel intel = new Intel();
Result result = intel.search("my-api-key", "test");
//Result result = intel.search("my-api-key", "test", 0, 2);
System.out.println(result);
```

To run advanced search based on parameters
(find all MD5 samples connecting to a domain and determined as malicious):

```java
import deepviz.intel.input.AdvancedSearchInput;
import deepviz.intel.Intel;
import deepviz.Result;

import java.util.ArrayList;
import java.util.List;

List<String> domains = new ArrayList<String>();
domains.add("justfacebook.net");

AdvancedSearchInput input = new AdvancedSearchInput();
input.setDomain(domains);
input.setClassification("M");

Result result = intel.advancedSearch("my-api-key", input);
System.out.println(result);
```