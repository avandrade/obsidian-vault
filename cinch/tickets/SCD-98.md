SCD - 98 - [SPIKE] What can Segment Profile API give us (cinch)?

### links:
- miro - https://miro.com/app/board/uXjVOtipkNM=/?moveToWidget=3458764527631460560&cot=14
- jira - https://cinch.atlassian.net/browse/SCD-98
- confluence - TODO
- https://segment.com/docs/personas/
- https://segment.com/docs/personas/personas-gdpr/

### Personas Overview
Personas is a powerful personalisation platform that helps you create customer profiles.
With personas you can build enrich and activate audiences across marketing tools

#### What can you do with personas
- Create unified customer profiles
	- it uses [[segment identity Resolution]] to take vent data across different channels and merge them into a complete user/account level profiles
	- this gives your org a sigle view of your customer base
- Enrich profiles with new traits
	- add detail to user profiles with new traits, and use them to power persnalised marketing campaigns. You can add new traits to your user or account profiles in segment usign:
		- computed traits: Use the Personas drag-and-drop interface to build a per-user (B2C) or per account (B2B) metrics on user profiles (e.g. "lifetime value" or "lead score")
		- SQL traits: Run custom queries on you DW usign the Personas SQL editor and import the results into Segment. With SQL traits, you can pull rich, uncaptured user data back into Segment
- Build Audiences
	- Create a list of users or accounts that match a specific criteria. For example, after creating an "innactive accounts" audience that list paid accoutns wit no login in 60 days, you can push the audience to your marketing and analytics tools.
- Sync audiences to marketing tools
	- Once you create your computed Traits and Audiences, Personas send them to your Segment destinations in just a few clicks. You can use these Traits and Audiences to personalise messages accros channels, optimise ad spend, and improve targeting. You can also use the Profile API to build in-app and onsite personalisation. Learn more about using Personas data and Profile API

#### Personas core components
The main parts of Personas are you Personas space(s), Audiences, the profile explorer and Traits

- Personas Spaces
	- A space is a separate Personas environment. There are 2 main reasons you might use spaces
		- To separate your dev and prod environmets (highly recommended)
		- To separate environements for distinct teams or geographical regions
- Audiences
	- An audiece is a list of either users or accounts that match a specific criteria. For example, Sement's Marketing team might build an active signups audience for an email marketing campaign. This Audience could contain all users who signed up in the last seven days and added a source within seven days of signing up.
- Profile Explorer and Traits
	- The profile explorer is a single view of your customers in Personas. Traits are users or accounts atributes that you can see inthe Profile Explorer
		- The Profile Explorer contains all data that you have on a user, from their event history to their traits and identifiers
		- Computed Traits are per-user or per-account traits that you create or "compute" in Personas using drag-and-drop interface. When you build a computed trait, Personas adds it to relevant user profiles. Personas recomputes these traits once an hour to make sure they are always accurate
		- SQL Traits are per-user or per-account traits that you create by running queries against your DW, which can include data not captured using segment implementation. Segment imports the results into Personas, and appends these traits to the user profile. Personas recomputes these traits every 12 hours to ensure accuracy.
		- Custom Traits are user or accoutn properties collected from all events you send to Personas. For example you can add any properties that you send as part of your track calls or identify calls (emails, first_name, last_name) as custom traits. You can view them in the Profile explorer and use them in your Audiences, Computed Traits and SQL Traits

### Personas Quickstart Guide
#### Personas Configuration Requirements
Personas is and add-on product that works alongside the core Segment connections product. Before you set up Personas, you must first set up and configure Segment Conections

To configure and use Personas, you need the following:
1. A Segment account and Workspace
2. Events flowing into connections from you digital properties where most of your valuable user behaviour accurs
3. Personas Administrator access. You must be either a workspace admin, or a workspace user with Personas Admin access to setup Audiences and computed trait. You can check your permissions by navigating to access management

#### Step 1: Create a New Developer Space
When starting working with Personas, you should start by creating a "Developer" Personas  space. This is your experimental and test evironement while you learn more about how Personas work. You can validate and identify resolution is working correctly, test audiences and traits in the Dev space and then apply the change to your "Prod" environment. This two-space method prevents you from making untested configuration changes that immediately affect PROD data

#### Step 2: Invite teammates to your Personas Space
Invite teammates to your Personas dev space and rant them access to the space

#### Step 3: Connect Production Sources
1. From your Personas space, go to your Space settings and click sources
2. On the screen that appears, choose one or two production sources from your Connections workspace. We recomment connecting your prod website or App source as a great starting point.
Once you select sources, Segment starts a replay of one month historical data from these sources into your Personas space. we're doing this step first so you have some user data in Personas to build your first audiences nd computed Traits.

The replay usually takes several hours, but the duration will vary depending on how much data you have sent through these sources in the past one month. When the replay finishes you are notified in the Sources tab under settings

#### Step 4: Check your Profile data
Once the replay finishes, you can see the data replayed into Personas using the Profile Explorer. You should have a lot! The data should include information from multiple sources and multiple sessions, all resolved into a single profile user. Before continuing, check a few user profiles to make sure they show an accurate and recent snapshot of your users.

#### Step 5: Create an Audience
You can build an audience using any of source data that flows into your Personas space. To furhter verify your data, in this step create an Audience that you are familiar with and that you already have a rough idea of the size of. For example, you might know the number of new website user sign-ups in the last 7 days, if you've connected our productin website source to Personas.

The Audience Builder UI prompts you to filter your users using on specific behaviours that they performed. The audience in the example below is all the users who have performed the event `User Signed Up` at least one time in the last 7 days

#### Step 6: Connect the Audience to a Destination
Once you create your test audience, click `Select Destinations`. Personas guides you through configurations steps to setuo a destination for your audeince. 
The larger the audience you're creating, the longer it takes Personas to successfully compute te Audience. Te Audience page shows a status that indicates if the Audience overview, as in this example screenshot bel ow, the audience has 

#### Step 7: Validate taht your audience is appearing in your destination
Audiences are either sent to destinations as a boolean user-property (for example New_Users_7days=true or a user-list depending on what the destination supports)

The UI for the destination tools you send the audience data to are different, so the process of validating the audience varies per tool. However, the guiding principle is the same. You should be able to identify te full group of users who are members of your audience in your destination

#### Step 8: Create your production Space
Once you validate that your full audience is arriving in your destination, you are ready to create a Production space. we recommend you repeat the same steps outlined aove, focusing on your production use cases and data sources.

## Personas Identity Resolution
### Overview
#### Identity Graph
Identity Resolution sits at the core of Personas. The Segment Identity Graph merges the complete history of each customer into a single profile, no matter where they interact with your business. Identity Resolution allows you to understand a user's interaction across web, mobile, server and third party partner touch-points in real time using an online and offline ID graph with support from cookie IDs, device IDs, email, and custom external IDs. If you are sending the `group` call, you can also understand user behaviour at the account level.

Highlights
1. Supports existing data - no additional code or setup required
2. Supports all channels - stitches web + mobile + server + third party interactionsn into the same user
3. Supports anonymous identity stitching - by merging child sessions into parent sessions
4. Supports user:account relationship - for b2b companies, generates a graph of relationships between users and accounts
5. Realtime - merges realtime data streams, tested at 50,000 resolutions/sec with a P95 resolve duration of 7ms

Technical Highlights
1. Supports custom external IDs - bring you own external  IDs
2. Customisable ID Rules - allows you to enforce uniqueness on select external IDs and customise which external IDs and sources cause associations
3. Merge protection - automatically detects and solves identity issues such as non-unique anonymous IDs and the library problem using our priority trust algorithm 
4. Maintains persistent ID - multiple external IDs get matched to one peristent ID

### Identity Resolution Onboarding
Segment creates and merges user profiles based on space's identity resolution configuration. Segment searches for identifiers such as userId, anonymousId and email on incoming events and matches them to existing profiles or creates new profiles. These identifiers display in the identities tab of a User Profile in the User Explorer.

#### Flat matching logic
When Personas receive a new event, Segment looks for profiles that match any of the identifiers on the event.
Based on teh existence of a match, one of the three actions can occur

1. **Create a new profile** when there are no pre-existing profiles that have matching identifiers to the event, Segment creates a new user profile
2. **Add to existing profile** When there is one profile that matches all identifiers in an event, segment attemps to map the traits, identifiers and events on the call to that existing profile. If there is an excess of any identifier on the final profile, Segment defers to the Identity Resolution rules outlined below
3. **Merge existing existing profiles** When there are multiple profiles that match the identifiers in an event, Segment checks the Identity resolution rules outlined below and attempts to merge profiles

#### Identity Resolution Settings
Identity Admins should first configure Identity Resolution Settings to protect the identity graph from inaccurate merges and user profiles.

During the space creation process, the first step is to choose an Identity Resolution cnfiguratio. If this is your first space, you have the option to choose a segment suggested Out-of-the-box configuration or a custom Identity Resolution setup. All other spaces have a third option of importing setting from a different space
....
when you choose the limit on an identifier, ask the following questions about each of the identifier you send to segment:
1. Is it an immutable ID? An immutable ID such as user_id, should have 1 ever per user profile
2. Is it a constantly changing ID? A constantly changing ID, such as anonymous_id or ga_client_id, should have a short sliding window, such as 5 weeks or 5 months, depending on how often your application automatically logs the user
3. Is it an ID that updates ona yearly basis? Most customers will have around 5 emails or devices at any one time, but can update these over time. For identifiers like email, android.id, or ios.id, Segment recommends a longer limit like 5 annually.

**Priority**
Segment considers the priority of an identifier once that identifier exceeds the limit on the final profile

### Identity Resolution Use-Cases
The Personas Identity Resolution feature helps to create a unified view of the user across devices, apps, and unique identifiers. Identity Resolution is critical in understanding the customer journey at multiple touch points, which allows brands to deliver personalised experiences to its customers at scale.

**Anonymous to known Identification**


.......

skipped in the interest of time
........

## Personas Profile API
The Segment Profile API provides a single API to read user-level and account level customer data. Segment now allows you to query the entire user or account object programmatically, including the external_ids, traits, and events that make up a users's journey through your product

You can use this API to:
- Build an in-app recommendation engine to show users or acounts the last five products they viewed but didn't purchase
- Empower your sales and support associates with the complete customer context by embedding the user profile in third-party tools like zendesk
- power personalised marketing campaigns by enriching dynamic / custom properties with profile traits in marketing tools like Braze
- qualify leads faster by embedding the user event timeline in Salesforce

## Using your Personas data
You can send your Personas Computed Traits and Audiences to your segment Destination, which allows you personalise messages across chanels, optimise ad spend and improve targeting. This page provides overview of different ways to activate Personas data in Segment Destinations

```
Tip! You can use the Personas Profile API to activate Personas data programmatically.
```

### Compatible Personas Destinations
The table below includes the most important Personas Destinations the we support today, and which we consider industry-standard tool that most business use for personalisation and marketing. (this list does not include warehouses)

....
the table went here
....

In addition to the most popular Personas Destinations, Segment  supports additional Destinations you can use in conjnction with Personas. These are the full list of Destinations that are compatible with Segment.

Personas sends computed traits and audiences to destinations in different way depending on whether the destination is an Event or List type
- Computed Traits are always sent to Event destination either using Identify call for user trait, a group call for account-level computed trait or a track event
- With Audiences, Personas sends the audience either as a boolean (true or false) user property to Event Destinations, or a list to List Destinations. If you are B2B company creating account audiences (where each account represents a group of users, like employees at a business) and sending them to list destination, Personas sends the list of all users within an account that sastisfies the audience criteria 

Event Destinations
**Event Destinations and Computed Traits** Computed traits can only be sent to Event Destinations. When Personas sends a computed trait to an Event destination, it uses an identify call to send user traits or a group call to send account-level computed traits.


**Event Destinations and Audiences**
- **`identify`  call as a user trait** When you use identify calls, the trait name is the snake_cased version of the audience name you provided, and the value is "true" if the user is part of the audience.  For example, when a user first completes an order inthe last 30 days, Segment sends an identify call with property `order_completed_last_30days: true`, and when this user no longer satisfies that criteria (for example if the 30 days elapses and they haven't completed another order), Segment sets that value to False
- **`track` call as two events** `Audience Entered` and `Audience Exited`, with the event property `order_completed_last_30days` equal to true and false, respectively.

### List Destinations
List destinations can only receive Audiences and cannot receive computed traits
- User-Level Audiences: A list of users that belong to an audience
- Account-level Audiences: A list of users within an account that satisfies the audience criteria
When syncing to List destination Personas uploads lists of users directly to the destination. When you first create an audience, Segment uploads the entire list of audience users to the destination. Later syncs only uploads the users that have been added or removed since the last sync.

User list destinations can have individual limits on how often Segment can sync with them. For example, an AdWorks audience is updated once every six hours, because that is what AdWorks recommends

### What do the payloads look like for Personas data?
The payload sent from your Personas space to your destinations will be different depending on if you configured the destination to receive identify or track calls, and whether the payload is coming from a computed trait or audience.
As a reminder, Identify calls usually  update a trait on a user profile or table, whereas track calls send a point-in-time event that can be used as campaign trigger or detailed record of when a user's audience membershio or computed trait value was calculated.

#### Computed Trait Generated Events
`Identify` events generated by a computed trait have the trait name set to computed trait value
`track` events generated by a computed trait have a key for the trait name and a key for the computed trait value. The default event name is Trait computed, but you can change it.

#### Audience generated events
`Identify` events generated by an audience have the audience key set to true of false based on whether the user is entering or exiting the audience

`Track` events generated by an audience have a key for the audience name and a key for the audience values

#### Additional identifiers
Personas has a flexible flexible identity resolution layer that allows you to build user profiles based on multiple identifiers like user_id, email, mobile advertisingId, etc.Howerver different destinations may require different jeys , so theu can to their own matching and identification. For example Zendesk reequires that you include name property. Personas inludes logic to automatically enrich payloads going to there destinations with required keys.

### Multiple identifier of the same type
You might also see that profiles that have multiple values for the same externl_id type, for example a profile might have multiple email addresses. When this happens, Personas sends one event per email for each audience or computed trait event. This ensures that all downstream email-based profiles receive the complete audience or computed trait.

### New external identifiers added to a profile
There are 2 situations when Personas sends an audience or computed trait to a destnation

The first is when the value of the trait or audience changes

The second, less common case is that Personas re-syncs audience or computed trait when a new external_id is added to a profile. For example, an e-c ommerce company has an anonymous visitor with a computed trait called `last_viewed_category = 'Shoes'`. That visitot then creates an account and an email address is added to the profile, Personas re-syncs the computed trait that includes an email to downstream tools. This allows the ecommerce company to start personolising the user's experience from a more complete profile

### Rate Limits on Personas Destinations


....
blah blah blah! not interested
....

## Personas and Warehouse
Personas provides a complete, up-to-date view of your users csutomer journey as it unfolds, and one of the best ways to understand the data produced by this journey is by analysing the data in your data warehouse using SQL.

With Personas, you can send computed Traits and Audiences to a data warehouse like Resdhift, BigQuery and Snowflake. This allows you to perform analysis and reporting around key customer audiences and campaigns, as well as set up your user data as imput into predictive models

Segment makes it easy to load your customer profile data into a clean schema so your analysts can help answer some of your toughest business questions.

### Identify calls for adiences
If you choose to send your personas data as identify call, Personas usually sends one call per user.

When you send audiences as an Identify call, Personas includes a boolean trait that matches the name of the audience. When a user enters an audience the boolean is set to true and when they exit the boolean is set to false.

### Identify call for computed traits
When you send computed traits as an identify call, Personas sends a similat call with the computed value for that trait. In the example below, the trait total_revenue_180_days includes the calculated value of 450.00

### Warehouse schema for Personas identify call
Personas identify calls appear in your data warehouse using a similar format as normal connections identify calls. Personas identyfy calls appear in two tables per Persona space
These tables are named with a prefix of personas_, then the Personas space name followed by the identifies or users.
The identifies table contains a record of every identify call, and the users table contains one record per user_id with the most recent value
The personas_ schema name is specific to the Personas space and cannot be modified.
Additional audiences and computed traits appear as additional coumns in these tables

`personas_default.identifies` 
`personas_default.users`

#### Track calls for audiences


## Computed Traits
Computed Traits allow you to quickly create user or account-level calculations that Segment keeps up-to-date over time.

These can be computations like the total_number_of_orders a customer has completed, the lifetime_revenue of a customer, the most_frequent_user to determine which user is most active in an account, or the unique_visitors_count to assess how many visitors from a single domain. These computations are based on your events and event properties that you are sending through Segment on the page and track calls.


### Types of Computed Traits
Personas currently supports the following types of computed traits
- Types of Computed Traits
	- Event Counter
	- Aggregation
	- Most Frequent
	- First
	- Last
	- Unique List
	- Unique List Count
-  Conditions
- Connecting your computed traits to a Destination
- Accessing your Computed Traits using the Prifiles API
- Downloading you Computed Traits as a CSV



### While playing with Segment and dev cinch website

- Events involved in searches 
	- Viewed "find_vehicle" Page
		- page_name -> find_vehicle
		- path/ -> find-vehicle
		- search -> ?bodyType=&colour=&doors=&driveType=&features=&fromEngineSize=-1&fromPrice=-1&fromYear=-1&fuelType=&make=Ford&mileage=-1&pageNumber=1&pageSize=32&seats=&selectedModel=Mondeo&sortingCriteria=2&tags=&toEngineSize=-1&toPrice=-1&toYear=-1&transmissionType=&useMonthly=false&variant=
		- url -> https://cambridge.preview.aws.cinch.co.uk/find-vehicle?bodyType=&colour=&doors=&driveType=&features=&fromEngineSize=-1&fromPrice=-1&fromYear=-1&fuelType=&make=Ford&mileage=-1&pageNumber=1&pageSize=32&seats=&selectedModel=Mondeo&sortingCriteria=2&tags=&toEngineSize=-1&toPrice=-1&toYear=-1&transmissionType=&useMonthly=false&variant=
	
	- Find Vehicle - Filter panel
		- path -> /find-vehicle
		- search -> ?bodyType=&colour=&doors=&driveType=&features=&fromEngineSize=-1&fromPrice=-1&fromYear=-1&fuelType=&make=BMW&mileage=-1&pageNumber=1&pageSize=32&seats=&selectedModel=3%20Series&sortingCriteria=2&tags=&toEngineSize=4049&toPrice=-1&toYear=-1&transmissionType=manual&useMonthly=false&variant=
		-  url -> https://cambridge.preview.aws.cinch.co.uk/find-vehicle?bodyType=&colour=&doors=&driveType=&features=&fromEngineSize=-1&fromPrice=-1&fromYear=-1&fuelType=&make=BMW&mileage=-1&pageNumber=1&pageSize=32&seats=&selectedModel=3%20Series&sortingCriteria=2&tags=&toEngineSize=4049&toPrice=-1&toYear=-1&transmissionType=manual&useMonthly=false&variant=

	- Find Vehicle - 2 free text input
		- path -> /find-vehicle
		- referrer -> https://cambridge.preview.aws.cinch.co.uk/my-profile
		-  search -> ?bodyType=&colour=&doors=&driveType=&features=&fromEngineSize=-1&fromPrice=-1&fromYear=-1&fuelType=&make=Skoda&mileage=-1&pageNumber=1&pageSize=32&seats=&selectedModel=Octavia&sortingCriteria=2&tags=&toEngineSize=-1&toPrice=-1&toYear=-1&transmissionType=&useMonthly=false&variant=
		-  url -> https://cambridge.preview.aws.cinch.co.uk/find-vehicle?bodyType=&colour=&doors=&fromEngineSize=-1&fromPrice=-1&fromYear=-1&fuelType=&make=Audi&mileage=-1&pageNumber=1&pageSize=32&seats=&selectedModel=A1&sortingCriteria=3&toEngineSize=-1&toPrice=-1&toYear=-1&transmissionType=&useMonthly=false&variant=
 
	- Find Vehicle - 1 free text input
		- path -> /find-vehicle
		- referrer -> https://cambridge.preview.aws.cinch.co.uk/my-profile",
		- search -> ?bodyType=&colour=&doors=&driveType=&features=&fromEngineSize=-1&fromPrice=-1&fromYear=-1&fuelType=&make=Skoda&mileage=-1&pageNumber=1&pageSize=32&seats=&selectedModel=Octavia&sortingCriteria=2&tags=&toEngineSize=-1&toPrice=-1&toYear=-1&transmissionType=&useMonthly=false&variant=
		-  url -> https://cambridge.preview.aws.cinch.co.uk/find-vehicle?bodyType=&colour=&doors=&driveType=&features=&fromEngineSize=-1&fromPrice=-1&fromYear=-1&fuelType=&make=Skoda&mileage=-1&pageNumber=1&pageSize=32&seats=&selectedModel=Octavia&sortingCriteria=2&tags=&toEngineSize=-1&toPrice=-1&toYear=-1&transmissionType=&useMonthly=false&variant="

	- Find Vehicle - 3 free text input


