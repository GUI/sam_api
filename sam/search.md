---
published: true
layout: default

---

As part of the next SAM quarterly release, the SAM API service will expand to include the functionality for Search.  The SAM Search API has been developed to mimic the Search functionality that is currently available from the SAM website.  
The Search API provides the opportunity to perform a search transaction in the following two options. Quick Search which takes a single search term provided by the user and compares it to a set of predefined database fields.  Advanced Search allows the user to search by entering a value that is then used to search a user selected predefined search category. 

### Searching SAM using the API

#### Quick Search:

The Quick Search functionality allows a user to enter a single term which is then queried against eight fields in the SAM database.  The results should return the same businesses, grantees, and other registrants in SAM that are found when you do a "Quick Search" on SAM itself.

Example: [https://api.data.gov/sam/v1/registrations?qterms=GSA](https://api.data.gov/sam/v1/registrations?qterms=GSA)

You can include spaces by putting them in double-quotes and URL-encoding them like so:

[https://api.data.gov/sam/v1/registrations?qterms="My%20Company"](https://api.data.gov/sam/v1/registrations?qterms="My%20Company")

The Search API will then query the SAM database and display any registrant that matches the user selected search term contained in any of the following fields: 

| Field                  |
|------------------------|
| Legal Business Name    |
| Doing-Business-As Name |
| DUNS                   |
| DUNS+4                 |
| CAGE Code              |
| DoDAAC                 |

Note that the search will add a wildcard to the end of any passed in quick search term. So ```qterms=Rob``` will match "Robot Co., Inc."

#### Advanced Search

The Advanced Search functionality allows a user to enter a criteria or value and a category which is then used to query the database to return all registrations that match that selection criteria.  In Advanced Search users are able to string multiple criteria and categories to better refine their search and return a more specific list of registration.

Advanced Search uses the same ```qterms``` construct that we used for quick search, but follows more closely to the [Lucene-based syntax](http://lucene.apache.org/core/2_9_4/queryparsersyntax.html) for querying by specific terms. It's not exactly the same, but should be familiar for anyone who's used to that syntax.

A basic example, it you want to make sure you're searching only legal name, you can run the following search:

[https://api.data.gov/sam/v1/registrations?qterms=(legalBusinessName:incorporated)](https://api.data.gov/sam/v1/registrations?qterms=(legalBusinessName:incorporated))

The search terms will be in parentheses. You can AND and OR search terms by putting the terms ``` AND ``` or ``` OR ``` between terms, separated by spaces. (Eventually, they will be separated by ```+``` signs because of some ambiguities in the syntax, but for the moment, use spaces.)

[https://api.data.gov/sam/v1/registrations?qterms=(legalBusinessName:incorporated) AND (naicsLimitedSB:12345)](https://api.data.gov/sam/v1/registrations?qterms=(legalBusinessName:incorporated) AND (naicsLimitedSB:12345))

#### Advanced search fields

The following are the fields you can search for using Advanced Search. Note that, where appropriate, we have used the same field name as the JSON output from the detailed results of the API.

| Functional Field               | URL Search Field            | Possible Search Terms             |
|--------------------------------|-----------------------------|-----------------------------------|
| Legal Business Name            | legalBusinessName           |                                   |
| Doing Business As Name         | doingBusinessAs             |                                   |
| CAGE code                      | cage                        |                                   |
| DUNS number                    | duns                        |                                   |
| Physical Address City          | samAddress.city             |                                   |
| Physical Address Country       | samAddress.country          |                                   |
| State                          | samAddress.stateOrProvince  |                                   |
| Zip Code                       | samAddress.zip              |                                   |
| Congressional District         | congressionalDistrict       |                                   |
| NAICS - Limited SB             | naicsLimitedSB              |                                   |
| NAICS - Any Size               | naicsAnySize                |                                   |
| Minority Owned Business        | minorityOwned               | true or false                     |
| Women Owned Business           | womenOwned                  | true or false                     |
| Veteran Owned Business         | veteranOwned                | true or false                     |
| Service Disabled Veteran Owned | serviceDisabledVeteranOwned | true or false                     |
| SBA Certified 8A Program       | sba8AProgram                | true or false                     |
| SBA Certified Hubzone Program  | sbaHubzoneProgram           | true or false                     |
| Ability 1 Certified            | ability1                    | true or false                     |
| Purpose of Registration        | purpose                     | <ul><li>Z1 - Federal AssistanceAwards</li>     
                                                                 <li>Z2 - All Awards</li>
                                                                 <li>Z4 - AssistanceAwards & IGT</li>
                                                                 <li>Z5 - All Awards & IGT</li></ul>    |
| Registration Status            | registrationStatus          | <ul><li>A - Active</li>
                                                                 <li>W - Work in Progress</li>
                                                                 <li>S - Submitted</li>
                                                                 <li>I - Inactive</li></ul>                      |
| Disaster Response Contractor   | disasterResponse            | true or false                     |




