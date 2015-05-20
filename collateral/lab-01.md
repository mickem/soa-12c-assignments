# 1: Start Server
Start the built-in weblogic server and display the various admin GUIs like enterprice manager home page.
- [http://Localhost:7101/em](http://Localhost:7101/em)
- [http://Localhost:7101/console](http://Localhost:7101/console)
- [http://Localhost:7101/sbconsole](http://Localhost:7101/sbconsole)

Key      | Value
-------- | --------
Password | welcome1

## 1.1 Questions
- Where can you find the port for the admin server?
- Where is the server installed? (Which folder)
- Is the server a good example of a modern web UI?Motivate why/why-not

# 2: Create application
Create a "SOA Application" in JDeveloper

## 2.1 Details

Key              | Value
---------------- | ----------------------------------------------------------------
Application Name | SoaLabApp01
Directory        | C:\JDeveloper\mywork\soa-12c-assignments\SoaLabApp01
Application      | Package Prefix
Project Name     | SoaLabProj1
Directory        | C:\JDeveloper\mywork\soa-12c-assignments\SoaLabApp01\SoaLabProj1
Features         | SOA Suite
Composite Name   | Sample1-001
Start from:      | Empty Composite

## 2.2 Instructions
- 2.1 Create a new SOA Application in JDeveloper

## 2.3 Questions
- What's the main issue with the maven pom.xml file?
- What do you actually have now?
- What are the difference between the exposed services and external references panes?

# 3: Create a purchase order XSD
Create a XSD for storing the PO data (collateral/sample-order.xml):

## 3.1 Details

```
<?xml version="1.0" encoding="UTF-8" ?>
<orders xmlns="http://schemas.medin.name/wicked/soa/lab/01/orders">
  <order number="123" date="2015-05-20T01:01:01">
    <row>
      <item>76042: The Shield Helicarrier</item>
      <amount>10</amount>
      <price>3499,00</price>
    </row>
    <row>
      <item>10234: Sydney Opera House</item>
      <amount>7</amount>
      <price>3199,00</price>
    </row>
    <row>
      <item>10234: The Tumbler</item>
      <amount>2</amount>
      <price>2199,00</price>
    </row>
  </order>
</orders>
```

## 3.2 Instructions
- 3.0 Create a new schema file and edit it by hand

## 3.1 Questions
- Which is best source view or editor? (and why)
- What is best attributes or elements? (and why)
- Where is the schema located?

# 4: Create read adapter and write adapter
Add two adapters, one for reading and one for writing the payload. Read the file as XML using the schema we build previously and write the file to a json file (po.json)

## 4.1 Details

Key           | Value
------------- | ---------------------------------------------
Object        | Read Adapter
Name          | ReadOrders
Interface     | Define later
JNDI Name     | eis/FileAdapter
Operation     | Read
Physical Path | c:\tmp\orders\in (or /tmp/orders/in)
Pattern       | po*.xml
Schema        | Use schema from #3
Object        | Write Adapter
Name          | WriteOrders
Operation     | Write (append)
Physical Path | c:\tmp\orders\out (or /tmp/orders/out)
Filename      | pos.json
Append        | Yes
Schema        | Build schema from sample-po.json sample file.

## 4.2 Instructions
- 4.0 Create file read adapter
- 4.1 Create file write adapter from json payload

## 4.3 Questions
- What the difference between the inbound JCA and outbound JCA files?
- What the problem with how our path is specified?
- What does the JNDI name reference?

# 5: Wire them together with BPEL
Create a BPEL component with a receive, transformation and an invoke wiring them together.

## 5.1 Details

Key             | Value
--------------- | ---------------------------------
Object          | Receive
Name            | ReadOrder
Variable        | ReadOrder_Read_InputVariable
Create Instance | Yes
Object          | Xform
Name            | XFormOrders
Mapping         | Map the message using drag'n'drop
Object          | Invoke
Name            | WriteOrder
Variable        | WriteOrder_Write_InputVariable

## 5.2 Instructions
- 5.0 Added BPEL process to composite
- 5.5 Fixed bug in transform
- 5.2 Added variables and recieve/invoke
- 5.3 Added the xform
- 5.4 Used automapper to map the flow
- 5.5 Fixed bug in xform

## 5.3 Questions
- Why is editing the header discouraged?
- Why are the xpaths for the row elements not absolute?
- Why was your mapping not the same as the one in git?

# 6: Test the flow
Use both the JDeveloper test client and Enterprise Manager test client to test the flow.

## 6.1 Details
N/A

## 6.2 Instructions
- 6.0 Open EM
- 6.1 Navigate to flow
- 6.2 Use the test Features
- 6.3 Check the resulting output file

## 6.3 Questions
- What happens with invalid files?
- What's the problem with appending to a JSON file?
- What was the bug in the mapping?

# 7: Add a mediator and expose a WS
Add a mediator to the composite and expose it as a web service.

## 7.1 Details

Key      | Value
-------- | ------------------------------------------------------------
Name     | SubmitOrders
Template | Synchronous
Input    | Use the orders from the po (not the json)
Output   | Default ({http://xmlns.oracle.com/singleString}singleString)
Logic 1  | Copy the payload
Logic 2  | Add extra routing rule (echo) to create a reply)
Logic 3  | Add assign for return data: Wicked
Logic 4  | No 'Wicked' is not XML

## 7.2 Instructions
- 7.1 Wired mediator to BPEL process
- 7.2 Added assign in mediator
- 7.3 Added return message

## 7.3 Questions
- Can the mediator do anything BPEL cannot?
- Which test client is the best? (and why)
- Is the mediator different in the flow trace? (and if so why?)

# 8: Test the flow
Now use Ems built-in test client to test the flow Then use the JDeveloper version

## 8.1 Questions
- Why was there no test button before?
- What does save payload do?
- Why is the flow trace different between the mediator and BPEL?

# 10: Create OSB Application and Project
Create an application in JDeveloper for OSB and add a OSB Project.

# 10.1 Details

Key                | Value
------------------ | ----------------------------------------------------
Item               | Service Bus Application
Name               | OsbLabApp01
Template           | Empty OSB Project
Directory          | C:\JDeveloper\mywork\soa-12c-assignments\OsbLabApp01
Application Prefix |
Item               | Service Bus Project
Name               | OsbLabProj01
Features           | Service Bus

# 10.2 Instructions
- 10.0 Created OSB Project
- 10.1 Added OSB Project

# 11: Add a Business Service
Add a business service to our flow.

## 11.1 Details

Key          | Value
------------ | --------------------------------
Object       | Add    New HTTP
Name         | OrdersBS
WSDL         | Import from EM
Endpoint URI | Set this to WSDL (without ?wsdl)

## 11.2 Instructions
- 11.0 Imported WSDL from server
- 11.1 Added business service

## 11.3 Questions
- Why is there no source view in OSB?
- What the difference between a business service and a WSDL?
- Should we be worried about using git here?

# 12: Create a pipeline
Create a pipeline and expose it as a proxy service.

## 12.1 Details

Key                     | Value
----------------------- | --------------------
Service Name            | Orders
WSDL                    | SubmitOrders_ep.wsdl
Expose as Proxy Service | Yes
Proxy Service Name      | OrdersProxyService

## 12.2 Instructions
- 12.0 Added a pipeline
- 12.1 Added proxy service

## 12.3 Questions
- What can we configure on the Proxy?
- Which is better: OSB, Mediator or BPEL?

# 13: Test the flow end-to-end
Run the flow via WS from OSB and file and make sure everything is working as expected. Questions

# 13.1 Questions
- Is this simple or is it hard?
- Do we know any better way to achieve this?
- Where is the major pain points?

# 14 Bonus task
Create a similar flow via the sbconsole
- Can we do anything in JDeveloper that we cannot do in the web UI?
- How can we add throttling to our flow?
- Which is easier: JDeveloper or web?
