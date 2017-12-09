# Salesforce Outbound Message

Here you will find the following details about Salesforce Outbount Message.
<ol type="1">
<li>What is a Salesforce Outbound Message?</li>
<li>Setup an Endpoint</li>
<li>How to setup a Salesforce Outbound Message?</li>
<li>How to Test and Monitor a Salesforce Outbound Message?</li>
<li>Considerations of Salesforce Outbound Messages</li>
<li>Useful Resources</li>
</ol>

## (1) What is a Salesforce Outbound Message?
<ol type="a">
<li>Salesforce outbound message allows you to specify fields that changes within Salesforce and send those fields to a designated external endpoint/server without writing a single line of code.</li>
<li>It is a declarative (point and click) feature and does not need any programmatic skills.</li>
<li>It uses Asynchronous communication.</li>
<li>It uses SOAP(WSDL) protocol.</li>
<li>It creates response in XML data format.</li>
<li>It resides under Automation Tool.</li>
<li>It is part of Workflow Rules and Approval Processes.</li>
<li>Salesforce has a special permission <b>"Send Outbound Messages"</b> on Profile and Permission Sets. This permission allows user to send outbound messages to an external Web service API.</li>
</ol>

## (2) Setup an Endpoint
<ol type="a">
<li>You can create a Listener in any programming language (e.g. Java, .Net, PHP etc.). In other words you setup an endpoint where the Salesforce Outbound Message will send the message in XML.</li>
<li>For Testing purpose you can use following publicly available services. Most of them are free! In this example <b>"Integration Playground by Salesforce"</b> has been used for testing the outbound message.
<ul>
<li>Integration Playground by Salesforce (http://intg-playground.herokuapp.com/sfdc/omlistener)</li>
<li>PutsReq (http://putsreq.com/)</li>
<li>RequestBin (https://requestb.in/)</li>
<li>PostBin (http://postb.in/)</li>
<li>SOAP UI (On-Premise, Need to download the software to use it.)</li>
<li>Burp Suite (https://security.secure.force.com/security/tools/webapp/burpabout)</li>
<li>Runscope (https://www.runscope.com/)</li>
</ul>
</li>
</ol>

## (3) How to setup a Salesforce Outbound Message?
<ol type="a">
<li>Create an Outbound Message</li>
Go to = <b>Setup > App Setup > Workflow & Approvals > Outbound Messages</b>
<img src="supportedimages/image1.png" /> <br/>
<img src="supportedimages/image2.png" />
<li>Create a Workflow Rule "or" an Approval Process</li>
Create a workflow rule "or" an approval process and add an Outbound Message as an action.
<img src="supportedimages/image3.png" />
</ol>

## (4) How to Test and Monitor a Salesforce Outbound Message?
<ol type="a">
<li>Here in this example, an Account record has been used to test the Outbound Message</li>
<li><img src="supportedimages/image4.png" /></li>
<li>Go to the URL from the <b>"Integration Playground"</b>
<li><img src="supportedimages/image5.png" /></li>
<li><b>Salesforce has got Acknowledgement (Positive Response)</b></li>
<li><img src="supportedimages/image6.png" /></li>
<li>Check the Outbound Message Delivery Status <br/>
Go to = <b> Setup > Administration Setup > Monitoring > Outbound Messages </b>
</li>
<li><img src="supportedimages/image7.png" /></li>
<li><b>Salesforce has NOT Acknowledgement yet (Negative Response)</b></li>
<li><img src="supportedimages/image8.png" /></li>
<li>Check the Outbound Message Delivery Status <br/>
Go to = <b> Setup > Administration Setup > Monitoring > Outbound Messages </b>
</li>
<li><img src="supportedimages/image9.png" /></li>
<li><img src="supportedimages/image10.png" /></li>
</ol>

## (5) Considerations of Salesforce Outbound Messages
<ol type="a">
<li>After getting Outbound Message XML response by Salesforce, if you want to update a record back into Salesforce then you must select "Send Session ID" checkbox when you create an Outbound Message in Salesforce</li>
<img src="supportedimages/image11.png" />

<li>The external server will get response of Outbound Message by Salesforce as in below XML format

```
<?xml version="1.0" encoding="UTF-8" standalone="no"?><soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <soapenv:Body>
  <notifications xmlns="http://soap.sforce.com/2005/09/outbound">
   <OrganizationId>ABC90000000gNcmEAE</OrganizationId>
   <ActionId>00190000000PGrlAAG</ActionId>
   <SessionId>ABC90000000gNcm!ARcAQJ.8aOC5VjE5D1VicpNZqe2PZe9Z7Fq0W4wEvF_xbmqyOs3V4QwJtUsPZ84vaw2dpHcHR5aooybd9C3sbet94cJujlCY</SessionId>
   <EnterpriseUrl>https://na1.salesforce.com/services/Soap/c/41.0/ABC90000000gNcm</EnterpriseUrl>
   <PartnerUrl>https://na1.salesforce.com/services/Soap/u/41.0/ABC90000000gNcm</PartnerUrl>
   <Notification>
    <Id>04l9000001olR84AAE</Id>
    <sObject xmlns:sf="urn:sobject.enterprise.soap.sforce.com" xsi:type="sf:Account">
     <sf:Id>0019000001KwyWtAAJ</sf:Id>
     <sf:BillingCity>San Francisco</sf:BillingCity>
     <sf:BillingCountry>United States</sf:BillingCountry>
     <sf:BillingPostalCode>CA 94105</sf:BillingPostalCode>
     <sf:BillingStreet>The Landmark @ One Market, Suite 300</sf:BillingStreet>
     <sf:Name>Salesforce.com Inc.</sf:Name>
    </sObject>
   </Notification>
  </notifications>
 </soapenv:Body>
</soapenv:Envelope>
```

</li>
<li>
The external server must send acknowledgement response to Salesforce that the message has been received successfully. The response XML format must be as below: 

```
Success / Positive acknowledgement 
----------------------------------
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
	<soapenv:Body>
		<notificationsResponse xmlns="http://soap.sforce.com/2005/09/outbound">
			<Ack>true</Ack>
		</notificationsResponse>
	</soapenv:Body>
</soapenv:Envelope>

Failure / Negative acknowledgement
----------------------------------
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
	<soapenv:Body>
		<notificationsResponse xmlns="http://soap.sforce.com/2005/09/outbound">
			<Ack>true</Ack>
		</notificationsResponse>
	</soapenv:Body>
</soapenv:Envelope>
```

</li>
<li>If the response / acknowledgement is a failure then you will see errors in the Outbound Message Delivery Status Page.</li>
<li>Go to = <b> Setup > Administration Setup > Monitoring > Outbound Messages </b></li>
<li>You will be getting errors as below:</li>
<ul>
<li>"org.xml.sax.SAXParseException: Content is not allowed in prolog."</li>
<li>"SOAP response was a nack"</li>
</ul>
</li>

<li>https://help.salesforce.com/articleView?id=workflow_om_considerations.htm&type=0</li>
<li><img src="supportedimages/image12.png" /></li>
</ol>

## (6) Useful Resources
<ol type="a">
<li>https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_om_outboundmessaging.htm</li>
<li>http://developer.force.com/cookbook/recipe/sending-outbound-messages-with-workflow</li>
<li>https://developer.salesforce.com/page/Outbound_Messaging</li>
<li>https://help.salesforce.com/articleView?id=workflow_om_considerations.htm&type=0</li>
<li>http://www.tgerm.com/2014/08/testing-soap-outbound-messages-without-failures-saxexception.html</li>
<li>http://www.simplysfdc.com/2016/04/testing-outbound-message-in-salesforce.html</li>
</ol>
