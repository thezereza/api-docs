# ABSOLVENTA RESTful API

## Data formats

Our RESTful API accepts two different XML based data formats
to manage your job offers headlessly: `Absolventa XML` and `HR-XML`, which
nowadays is sometimes called `HR Open Standards`.

* [Absolventa XML](absolventa_xml.md)
* [HR-XML](hr_xml.md)

## Authentication

Our RESTful API uses HTTP Basic Authentication. Your account manager will hand out you an API key. Please
provide it as username for HTTP Basic Authentication. Leave the password
blank or fill it with a fallback letter like <code>"X"</code>.

## Endpoints

* [List all published job offers](#list-published-job-offers)
* [List all draft job offers](#list-draft-job-offers)
* [List all expired job offers](#list-expired-job-offers)
* [Create a job offer using Absolventa XML](#create-a-job-offer-using-absolventa-xml)
* [Update a job offer using Absolventa XML](#update-a-job-offer-using-absolventa-xml)
* [Create a job offer using HR-XML](#create-a-job-offer-using-hr-xml)
* [Update a job offer using HR-XML](#update-a-job-offer-using-hr-xml)
* [End a job offer ](#end-a-job-offer)
* [Prolongate a job offer](#prolongate-a-job-offer)


### List published job offers

List all job offers that are available in our public search interface.

<table>
  <tbody>
    <tr>
      <td>Endpoint</td>
      <td>HTTP method</td>
      <td>Response Format</td>
    </tr>
    <tr>
      <td>/api/r/job_offers.xml</td>
      <td>GET</td>
      <td>Absolventa XML</td>
    </tr>
  </tbody>
</table>

#### Example

```
curl -i -H 'Accept: application/xml' -u 28a622e8ea6665433729932112d1d9cc:X https://www.absolventa.de/api/r/job_offers.xml
```

### List draft job offers

List all job offers that are drafts (⇔ unpublished) and therefore haven't been available to be listed in our publich search interface yet.

<table>
  <tbody>
    <tr>
      <td>Endpoint</td>
      <td>HTTP method</td>
      <td>Response Format</td>
    </tr>
    <tr>
      <td>/api/r/job_offers/drafts.xml</td>
      <td>GET</td>
      <td>Absolventa XML</td>
    </tr>
  </tbody>
</table>

#### Example

```
curl -i -H 'Accept: application/xml' -u 28a622e8ea6665433729932112d1d9cc:X https://www.absolventa.de/api/r/job_offers/drafts.xml
```

### List expired job offers

List all job offers that are not available to be listed in our public search interface any longer because their ended_at value refers to a date in the past.

<table>
  <tbody>
    <tr>
      <td>Endpoint</td>
      <td>HTTP method</td>
      <td>Response Format</td>
    </tr>
    <tr>
      <td>/api/r/job_offers/expired.xml</td>
      <td>GET</td>
      <td>Absolventa XML</td>
    </tr>
  </tbody>
</table>

#### Example

```
curl -i -H 'Accept: application/xml' -u 28a622e8ea6665433729932112d1d9cc:X https://www.absolventa.de/api/r/job_offers/expired.xml
```

### Create a job offer using Absolventa XML

<table>
  <tbody>
    <tr>
      <td>Endpoint</td>
      <td>HTTP method</td>
      <td>Data Format</td>
    </tr>
    <tr>
      <td>/api/r/job_offers.xml</td>
      <td>POST</td>
      <td>Absolventa XML</td>
    </tr>
  </tbody>
</table>

#### Example

```
curl -i -X POST https://www.absolventa.de/api/r/job_offers.xml \
     -H 'Accept: application/xml' \
     -H 'Content-Type: application/xml' \
     -u 123:X \
     -d @- << EOF
           <job_offer>
             <title>Trainee Web Development (m/w/d)</title>
             <description>
               <![CDATA[Lorem Ipsum]]>
             </description>
             <application_url>https://www.example.com/jobs/123/apply</application_url>
             <job_offer_locations type='array'>
               <job_offer_location>
                 <street>Friedrichstrasse 67</street>
                 <zip>10117</zip>
                 <city>Berlin</city>
                 <country>Deutschland</country>
               </job_offer_location>
            </job_offer_locations>
           </job_offer>
           EOF
```

### Update a job offer using Absolventa XML

<table>
  <tbody>
    <tr>
      <td>Endpoint</td>
      <td>HTTP method</td>
      <td>Data Format</td>
    </tr>
    <tr>
      <td>/api/r/job_offers/{id}.xml</td>
      <td>PUT</td>
      <td>Absolventa XML</td>
    </tr>
  </tbody>
</table>

#### Example

```
curl -i -X PUT https://www.absolventa.de/api/r/job_offers/12345.xml \
     -H 'Accept: application/xml' \
     -H 'Content-Type: application/xml' \
     -u 123:X \
     -d @- << EOF
           <job_offer>
             <title>Trainee Web Development (m/w/d)</title>
             <description>
               <![CDATA[Lorem Ipsum]]>
             </description>
             <application_url>https://www.example.com/jobs/123/apply</application_url>
             <job_offer_locations type='array'>
               <job_offer_location>
                 <street>Friedrichstrasse 67</street>
                 <zip>10117</zip>
                 <city>Berlin</city>
                 <country>Deutschland</country>
               </job_offer_location>
            </job_offer_locations>
           </job_offer>
           EOF
```

### Create a job offer using HR-XML

<table>
  <tbody>
    <tr>
      <td>Endpoint</td>
      <td>HTTP method</td>
      <td>Data Format</td>
    </tr>
    <tr>
      <td>/api/r/hrxml/job_offers.xml</td>
      <td>POST</td>
      <td>HR-XML</td>
    </tr>
  </tbody>
</table>

#### Example

```
curl -i -X POST https://www.absolventa.de/api/r/hrxml/job_offers.xml \
     -H 'Accept: application/xml' \
     -H 'Content-Type: application/xml' \
     -u 123:X \
     -d @- << EOF
        <PositionOpening>
           <PositionRecordInfo>
             <Id idOwner="Threepwod Ventures">
               <IdValue>42</IdValue>
             </Id>
           </PositionRecordInfo>
           <PositionPostings>
             <PositionPosting>
               <Id validFrom="2014-01-12" validTo="2015-06-12">
                 <IdValue>42</IdValue>
               </Id>
               <Link><![CDATA[http://www.example.com/jobs/1]]></Link>
             </PositionPosting>
           </PositionPostings>
           <PositionProfile>
             <PositionDateInfo></PositionDateInfo>
             <PositionDetail>
               <Company>
                 <Name>Threepwood Ventures</Name>
               </Company>
               <PhysicalLocation>
                 <PostalAddress>
                   <CountryCode>DE</CountryCode>
                   <PostalCode>10405</PostalCode>
                   <Municipality>Berlin</Municipality>
                   <DeliveryAddress>
                     <StreetName>Melee-Island-Street</StreetName>
                     <BuildingNumber>42</BuildingNumber>
                   </DeliveryAddress>
                 </PostalAddress>
               </PhysicalLocation>
               <PhysicalLocation>
                 <PostalAddress>
                   <CountryCode>DE</CountryCode>
                   <PostalCode>63067</PostalCode>
                   <Municipality>Offenbach am Main</Municipality>
                   <DeliveryAddress>
                     <StreetName>Hölderlinstraße</StreetName>
                     <BuildingNumber>42</BuildingNumber>
                   </DeliveryAddress>
                 </PostalAddress>
               </PhysicalLocation>
               <PositionTitle><![CDATA[Trainee Web Development (m/w)]]></PositionTitle>
             </PositionDetail>
             <FormattedPositionDescription>
               <Name>Mode</Name>
               <Value>standard</Value>
             </FormattedPositionDescription>
             <FormattedPositionDescription>
               <Name>CompanyDescription</Name>
               <Value>Company description goes here!</Value>
             </FormattedPositionDescription>
             <FormattedPositionDescription>
               <Name>JobOfferPositionIds</Name>
               <Value>1</Value>
             </FormattedPositionDescription>
              <Name>JobOfferDescription</Name>
               <Value>
                 <![CDATA[Here is a nice description]]>
               </Value>
             </FormattedPositionDescription>
             <FormattedPositionDescription>
               <Name>TraineeGefluester</Name>
               <Value>1</Value>
             </FormattedPositionDescription>
             <HowToApply>
               <ApplicationMethod>
                 <InternetWebAddress><![CDATA[http://example.com/jobs/1/application]]></InternetWebAddress>
               </ApplicationMethod>
             </HowToApply>
            </PositionProfile>
          </PositionOpening>
          EOF
```

### Update a job offer using HR-XML

<table>
  <tbody>
    <tr>
      <td>Endpoint</td>
      <td>HTTP method</td>
      <td>Data Format</td>
    </tr>
    <tr>
      <td>/api/r/hrxml/job_offers/{id}.xml</td>
      <td>PUT</td>
      <td>HR-XML</td>
    </tr>
  </tbody>
</table>

#### Example

```
curl -i -X PUT https://www.absolventa.de/api/r/hrxml/job_offers/12345.xml \
     -H 'Accept: application/xml' \
     -H 'Content-Type: application/xml' \
     -u 123:X \
     -d @- << EOF
         <PositionOpening>
           <PositionRecordInfo>
             <Id idOwner="Threepwod Ventures">
               <IdValue>42</IdValue>
             </Id>
           </PositionRecordInfo>
           <PositionPostings>
             <PositionPosting>
               <Id validFrom="2014-01-12" validTo="2015-06-12">
                 <IdValue>42</IdValue>
               </Id>
               <Link><![CDATA[http://www.example.com/jobs/1]]></Link>
             </PositionPosting>
           </PositionPostings>
           <PositionProfile>
             <PositionDateInfo></PositionDateInfo>
             <PositionDetail>
               <Company>
                 <Name>Threepwood Ventures</Name>
               </Company>
               <PhysicalLocation>
                 <PostalAddress>
                   <CountryCode>DE</CountryCode>
                   <PostalCode>10405</PostalCode>
                   <Municipality>Berlin</Municipality>
                   <DeliveryAddress>
                     <StreetName>Melee-Island-Street</StreetName>
                     <BuildingNumber>42</BuildingNumber>
                   </DeliveryAddress>
                 </PostalAddress>
               </PhysicalLocation>
               <PhysicalLocation>
                 <PostalAddress>
                   <CountryCode>DE</CountryCode>
                   <PostalCode>63067</PostalCode>
                   <Municipality>Offenbach am Main</Municipality>
                   <DeliveryAddress>
                     <StreetName>Hölderlinstraße</StreetName>
                     <BuildingNumber>42</BuildingNumber>
                   </DeliveryAddress>
                 </PostalAddress>
               </PhysicalLocation>
               <PositionTitle><![CDATA[Trainee Web Development (m/w)]]></PositionTitle>
             </PositionDetail>
             <FormattedPositionDescription>
               <Name>Mode</Name>
               <Value>standard</Value>
             </FormattedPositionDescription>
             <FormattedPositionDescription>
               <Name>CompanyDescription</Name>
               <Value>Company description goes here!</Value>
             </FormattedPositionDescription>
             <FormattedPositionDescription>
               <Name>JobOfferPositionIds</Name>
               <Value>1</Value>
             </FormattedPositionDescription>
              <Name>JobOfferDescription</Name>
               <Value>
                 <![CDATA[Here is a nice description]]>
               </Value>
             </FormattedPositionDescription>
             <FormattedPositionDescription>
               <Name>TraineeGefluester</Name>
               <Value>1</Value>
             </FormattedPositionDescription>
             <HowToApply>
               <ApplicationMethod>
                 <InternetWebAddress><![CDATA[http://example.com/jobs/1/application]]></InternetWebAddress>
               </ApplicationMethod>
             </HowToApply>
            </PositionProfile>
          </PositionOpening>
          EOF
```

### End a job offer

If you want to unpublish a job offer before its natural end of runtime, you can perform a PUT request to the corresponding endpoint as shown below.
By this endpoint you can also edit the current end date to a new value. To achieve this, wrap your requested datetime wrapped in XML like this and send it
as data attached to your request:

<code>
  &ltended_at&gt2019-01-01&lt/ended_at&gt
</code>

Note that your requested date has to
be before the job offer's current value of ended_at.

<table>
  <tbody>
    <tr>
      <td>Endpoint</td>
      <td>HTTP method</td>
      <td>Data Format</td>
    </tr>
    <tr>
      <td>/api/r/job_offers/{id}/quit.xml</td>
      <td>PUT</td>
      <td>Absolventa XML or HR-XML</td>
    </tr>
  </tbody>
</table>

#### Examples

```
curl -X PUT -H 'Accept: application/xml' \
            -H 'Content-Type: application/xml'&#x000A; \
            -u 28a622e8ea6665433729932112d1d9cc:X \
            https://www.absolventa.de/api/r/job_offers/12345/quit.xml

curl -X PUT -H 'Accept: application/xml' \
            -H 'Content-Type: application/xml'&#x000A; \
            -u 28a622e8ea6665433729932112d1d9cc:X \
            -d '<ended_at>2019-07-28T14:27:31+02:00</ended_at>' \
            https://www.absolventa.de/api/r/job_offers/12345/quit.xml
```

### Prolongate a job offer

Prior to the expiration date, the runtime of a <em>standard</em> job offer can be extended by another 12 months.

<table>
  <tbody>
  <tr>
    <td>Endpoint</td>
    <td>HTTP method</td>
    <td>Data Format</td>
  </tr>
  <tr>
    <td>/api/r/job_offers/{id}/prolongate.xml</td>
    <td>PUT</td>
    <td>Absolventa XML or HR-XML</td>
  </tr>
  </tbody>
</table>

#### Examples

```
curl -X PUT -H 'Accept: application/xml' \
            -H 'Content-Type: application/xml'&#x000A; \
            -u 28a622e8ea6665433729932112d1d9cc:X \
            https://www.absolventa.de/api/r/job_offers/12345/prolongate.xml
```
