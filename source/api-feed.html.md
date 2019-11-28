---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - xml
  - json

toc_footers:
  - <a href='mailto:info@jobylon.com?subject=PushAPI-credentials'>Email us to get your credentials</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

search: true
---

# Feed API

The Feed API allows you as a Jobylon-customer to retrieve job data in XML/JSON-format and display in your own interface. This is suitable for more advanced implementations than what our job widget can provide.

## Getting started

Our customer support can set up feeds on request. Once configured, you will recieve a URL for the specific feed. The default content format is XML, but by appending ?format=json to any URL the content will be formatted in JSON.

## Example implementations

- [Futurice careers](https://www.futurice.com/careers)

# Formatters

Depending on your usage scenario we can provide different formatters. The basic formatter should suffice most custom implementations, but we also provide prepared feeds for services such as Monster and Linkedin.

## Basic

```xml
> XSD Schema:
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="job">
    <xs:complexType>
      <xs:sequence>
        <xs:element type="xs:int" name="id"/>
        <xs:element name="categories">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="category">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element type="xs:byte" name="id"/>
                    <xs:element type="xs:string" name="text"/>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element name="company">
          <xs:complexType>
            <xs:sequence>
              <xs:element type="xs:string" name="industry"/>
              <xs:element type="xs:string" name="slug"/>
              <xs:element type="xs:string" name="descr"/>
              <xs:element type="xs:string" name="logo"/>
              <xs:element type="xs:string" name="website"/>
              <xs:element type="xs:string" name="cover"/>
              <xs:element type="xs:short" name="id"/>
              <xs:element type="xs:string" name="name"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element name="contact">
          <xs:complexType>
            <xs:sequence>
              <xs:element type="xs:string" name="name"/>
              <xs:element type="xs:string" name="photo"/>
              <xs:element type="xs:string" name="email"/>
              <xs:element type="xs:string" name="phone"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element name="departments">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="department">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element type="xs:string" name="descr"/>
                    <xs:element type="xs:short" name="id"/>
                    <xs:element type="xs:string" name="name"/>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element type="xs:string" name="title"/>
        <xs:element type="xs:string" name="slug"/>
        <xs:element type="xs:string" name="descr"/>
        <xs:element type="xs:string" name="skills"/>
        <xs:element type="xs:string" name="function"/>
        <xs:element type="xs:string" name="experience"/>
        <xs:element type="xs:string" name="employment_type"/>
        <xs:element type="xs:date" name="from_date"/>
        <xs:element type="xs:date" name="to_date"/>
        <xs:element type="xs:string" name="language"/>
        <xs:element name="locations">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="location">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element type="xs:string" name="area_1_short"/>
                    <xs:element type="xs:string" name="text"/>
                    <xs:element type="xs:string" name="url"/>
                    <xs:element type="xs:string" name="place_id"/>
                    <xs:element type="xs:string" name="country"/>
                    <xs:element type="xs:string" name="area_1"/>
                    <xs:element type="xs:string" name="country_short"/>
                    <xs:element type="xs:string" name="city"/>
                    <xs:element type="xs:string" name="city_short"/>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element name="video">
          <xs:complexType>
            <xs:sequence>
              <xs:element type="xs:string" name="content"/>
              <xs:element type="xs:string" name="url"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element name="urls">
          <xs:complexType>
            <xs:sequence>
              <xs:element type="xs:string" name="ad"/>
              <xs:element type="xs:string" name="apply"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>

> XML Example:

<job>
   <id>4069</id>
   <categories>
      <category>
         <id>159</id>
         <text><![CDATA[Tech]]></text>
      </category>
   </categories>
   <company>
      <id>6</id>
      <slug><![CDATA[jobylon]]></slug>
      <logo><![CDATA[https://media-eu.jobylon.com/companies/company-logo/jobylon/social_squared_rgb.911b9470.png]]></logo>
      <cover><![CDATA[https://media-eu.jobylon.com/jobs/company-cover/full-stack-pythondjango-developer/cover.940fe1cc.jpg]]></cover>
      <industry><![CDATA[Startup]]></industry>
      <name><![CDATA[Jobylon]]></name>
      <descr><![CDATA[<p>Jobylon develops and offers a modern hiring software (SaaS) to companies who want to create and share <strong>beautiful job ads and manage their applicants with ease.</strong> The recruitment industry has stayed unchanged for too long whilst the need for a simple and efficient tool has increased.</p> <p>Jobylon was founded in 2011 and since then we've launched a number of exciting projects and ventures within the HR tech space. We've always had the same goal - <strong>to use technology and innovation to change and simplify what is today a traditional space.</strong></p> <p>During this time we've also been awarded as one of <strong>Sweden's hottest entrepreneurs</strong> and nominated for the <strong>Red Herring</strong> award - we've done this with a small but dedicated team!</p> <p>Today, we work with nearly <strong>1000 employers</strong> from more than <strong>80 countries worldwide</strong>, including amazing organisations such as 7-Eleven, Schibsted, Scandic Hotels, Vaimo, Bonnier and <a href="https://www.jobylon.com/testimonials/">many more!</a></p>]]></descr>
      <website><![CDATA[http://emp.jobylon.com/]]></website>
   </company>
   <contact>
      <name><![CDATA[Niklas Emanuelsson]]></name>
      <email><![CDATA[niklas@jobylon.com]]></email>
      <photo><![CDATA[https://media-eu.jobylon.com/main_contact/f299a79d289ca240b00bc57529552caa.0622460b.jpg]]></photo>
      <phone><![CDATA[+4670 487 25 18]]></phone>
   </contact>
   <departments />
   <title><![CDATA[Full-stack Python/Django developer]]></title>
   <slug><![CDATA[full-stack-pythondjango-developer]]></slug>
   <descr><![CDATA[<p>We're a growing start-up with big ambitions and even though we've come a long way, we have a lot of exciting challenges left to tackle! We are now looking for another <strong>dedicated, eager and passionate</strong> developer to further help us realise our vision. We have <a href="https://emp.jobylon.com/customers">amazing clients</a>, loads of ideas and an exciting roadmap, but mainly we're looking to change a traditional space and create value for our customers. <strong>That's why we need you!</strong></p> <p>On a day to day basis, you'll be involved in all phases of the software life cycle, from specification to design, implementation and deployment.</p> <p>You are keen to contribute with your own ideas and we expect that you'll grow into an important part in our expansion, owning and driving parts of the system.</p>]]></descr>
   <skills><![CDATA[<p>You are an experienced full-stack developer who have a genuine passion for web development and always looking to learn new technologies.</p> <p><strong>You build things daily with</strong></p> <ul> <li>Python and Django</li> <li>JavaScript/CoffeeScript/ES6</li> </ul> <p><strong>You know your way around</strong></p> <ul> <li>ElasticSearch</li> <li>Memcached and Redis</li> <li>MySQL/PostgreSQL</li> <li>RabbitMQ</li> </ul> <p><strong>Played a bit with</strong></p> <ul> <li>JavaScript MVC (Angular, Flux/React or similar)</li> </ul> <p><strong>And sometimes tamper with</strong></p> <ul> <li>LESS and CSS</li> </ul>]]></skills>
   <function><![CDATA[Software Development]]></function>
   <experience><![CDATA[Experienced]]></experience>
   <employment_type><![CDATA[Full-time]]></employment_type>
   <from_date>2016-04-21</from_date>
   <to_date>2019-12-31</to_date>
   <language><![CDATA[en]]></language>
   <locations>
      <location>
         <city_short><![CDATA[Stockholm]]></city_short>
         <area_1><![CDATA[Stockholms län]]></area_1>
         <postal_code><![CDATA[113 31]]></postal_code>
         <area_1_short><![CDATA[Stockholms län]]></area_1_short>
         <municipality><![CDATA[Stockholm]]></municipality>
         <country><![CDATA[Sweden]]></country>
         <url><![CDATA[https://maps.google.com/?q=H%C3%A4lsingegatan+49,+113+31+Stockholm,+Sweden&ftid=0x465f9d76eae53039:0x1005e3c61fb0a66]]></url>
         <place_id><![CDATA[ChIJOTDl6nadX0YRZgr7YTxeAAE]]></place_id>
         <text><![CDATA[Hälsingegatan 49, Stockholm, Sweden]]></text>
         <city><![CDATA[Stockholm]]></city>
         <postal_code_short><![CDATA[113 31]]></postal_code_short>
         <country_short><![CDATA[SE]]></country_short>
      </location>
   </locations>
   <video>
      <content />
      <url />
   </video>
   <urls>
      <ad><![CDATA[https://emp.jobylon.com/jobs/4069-jobylon-full-stack-pythondjango-developer/]]></ad>
      <apply><![CDATA[https://emp.jobylon.com/applications/jobs/4069/create/]]></apply>
   </urls>
</job>
```

```json

> JSON Example:

[
   {
   "id":4069,
   "categories":[
      {
         "category":{
            "text":"Tech",
            "id":159
         }
      }
   ],
   "company":{
      "logo":"https://media-eu.jobylon.com/companies/company-logo/jobylon/social_squared_rgb.911b9470.png",
      "slug":"jobylon",
      "descr":"<p>Jobylon develops and offers a modern hiring software (SaaS) to companies who want to create and share <strong>beautiful job ads and manage their applicants with ease.</strong> The recruitment industry has stayed unchanged for too long whilst the need for a simple and efficient tool has increased.</p>\n<p>Jobylon was founded in 2011 and since then we've launched a number of exciting projects and ventures within the HR tech space. We've always had the same goal - <strong>to use technology and innovation to change and simplify what is today a traditional space.</strong></p>\n<p>During this time we've also been awarded as one of <strong>Sweden's hottest entrepreneurs</strong> and nominated for the <strong>Red Herring</strong> award - we've done this with a small but dedicated team!</p>\n<p>Today, we work with nearly <strong>1000 employers</strong> from more than <strong>80 countries worldwide</strong>, including amazing organisations such as 7-Eleven, Schibsted, Scandic Hotels, Vaimo, Bonnier and <a href=\"https://www.jobylon.com/testimonials/\">many more!</a></p>",
      "website":"http://emp.jobylon.com/",
      "id":6,
      "industry":"Startup",
      "name":"Jobylon",
      "cover":"https://media-eu.jobylon.com/jobs/company-cover/full-stack-pythondjango-developer/cover.940fe1cc.jpg"
   },
   "contact":{
      "phone":"+4670 487 25 18",
      "email":"niklas@jobylon.com",
      "name":"Niklas Emanuelsson",
      "photo":"https://media-eu.jobylon.com/main_contact/f299a79d289ca240b00bc57529552caa.0622460b.jpg"
   },
   "departments":[

   ],
   "title":"Full-stack Python/Django developer ",
   "slug":"full-stack-pythondjango-developer",
   "descr":"<p>We're a growing start-up with big ambitions and even though we've come a long way, we have a lot of exciting challenges left to tackle! We are now looking for another <strong>dedicated, eager and passionate</strong> developer to further help us realise our vision. We have <a href=\"https://emp.jobylon.com/customers\">amazing clients</a>, loads of ideas and an exciting roadmap, but mainly we're looking to change a traditional space and create value for our customers. <strong>That's why we need you!</strong></p>\n<p>On a day to day basis, you'll be involved in all phases of the software life cycle, from specification to design, implementation and deployment.</p>\n<p>You are keen to contribute with your own ideas and we expect that you'll grow into an important part in our expansion, owning and driving parts of the system.</p>",
   "skills":"<p>You are an experienced full-stack developer who have a genuine passion for web development and always looking to learn new technologies.</p>\n<p><strong>You build things daily with</strong></p>\n<ul>\n<li>Python and Django</li>\n<li>JavaScript/CoffeeScript/ES6</li>\n</ul>\n<p><strong>You know your way around</strong></p>\n<ul>\n<li>ElasticSearch</li>\n<li>Memcached and Redis</li>\n<li>MySQL/PostgreSQL</li>\n<li>RabbitMQ</li>\n</ul>\n<p><strong>Played a bit with</strong></p>\n<ul>\n<li>JavaScript MVC (Angular, Flux/React or similar)</li>\n</ul>\n<p><strong>And sometimes tamper with</strong></p>\n<ul>\n<li>LESS and CSS</li>\n</ul>",
   "function":"Software Development",
   "experience":"Experienced",
   "employment_type":"Full-time",
   "from_date":"2016-04-21",
   "to_date":"2019-12-31",
   "language":"en",
   "locations":[
      {
         "location":{
            "city":"Stockholm",
            "municipality":"Stockholm",
            "postal_code_short":"113 31",
            "country_short":"SE",
            "area_1_short":"Stockholms l\u00e4n",
            "city_short":"Stockholm",
            "area_1":"Stockholms l\u00e4n",
            "url":"https://maps.google.com/?q=H%C3%A4lsingegatan+49,+113+31+Stockholm,+Sweden&ftid=0x465f9d76eae53039:0x1005e3c61fb0a66",
            "place_id":"ChIJOTDl6nadX0YRZgr7YTxeAAE",
            "text":"H\u00e4lsingegatan 49, Stockholm, Sweden",
            "country":"Sweden",
            "postal_code":"113 31"
         }
      }
   ],
   "video":{
      "content":null,
      "url":""
   },
   "urls":{
      "apply":"https://emp.jobylon.com/applications/jobs/4069/create/",
      "ad":"https://emp.jobylon.com/jobs/4069-jobylon-full-stack-pythondjango-developer/"
   }
}
]
```

The basic formatter is suitable for most custom implementations.

**Fields**

| Name            | Type    | Description                      |
|-----------------|---------|----------------------------------|
| id              | integer | Job ID                           |
| categories      | array   | Categories added on job          |
| company         | object  | ...                              |
| contract        | object  | ...                              |
| departments     | array   | Departments added to job         |
| title           | string  | Job title                        |
| slug            | string  | Job slug, without complete URL   |
| descr           | string  | HTML-formatted string of text    |
| skills          | string  | HTML-formatted string of text    |
| function        | string  | Job function                     |
| experience      | string  | Experience level                 |
| employment_type | string  | Employment type                  |
| from_date       | string  | Job ad advertised from this date |
| to_date         | string  | Job ad advertised from to date   |
| language        | string  | Language used in job ad          |
| locations       | string  | Location of the job              |
| video           | string  | Job video                        |
| urls            | string  | URLs for ad and application      |

## CareerBuilder

...

## LinkedIn Limited Listings

...

## Monster

...