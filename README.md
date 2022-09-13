# From Paper to Pure Automation: How We Transformed Our Volunteer Onboarding Process

## Start Here
So whether you're here to learn or to plug-and-play this course into your own organization, we've got you covered. You can find everything you need in this repo! And if you have any questions or run into any snags, hit me up on Rocket.chat: **@leahjennings**.

### Components to Setup
1. Connection Opportunities/Type
2. System Communications
3. Workflows
4. Defined Types
5. Connection Opportunity Attributes
6. Triggers & Jobs
7. External Connection Signup
<!--
8. Metrics
9. Reporting Pages
-->

### Connection Opportunities/Type
Chances are you already have this created. However, we'll walk through setting this up even if you don't. There should be one connection type that encompasses _serving_, then as many Connection Opportunities as you have ministries. The way this solution is designed is that one connection opportunity is one ministry (Worship, Family Ministry, etc.).

#### 1. Create / Verify _Serving_ Connection Type
1. Navigate to People > Connections
2. Click on the _Options_ gear in the top-right hand corner
3. Verify that you have a Connection Type in the list that encompasses serving in your church (Volunteering, Calling, Serving, Involvement, etc.)
   - If you have one of those, great! _If not, create one and then follow the remaining steps. The following steps outline the customization needed for this solution. If a particular section isn't mentioned, its configuration isn't relevant to this process._
4. Check the _Enable Future Follow-Up_ checkbox
5. Add two new **Connection Request Attributes**
   1. Gender
       - Field Type: Single-Select (_built for the Unknown gender could be hidden_)
       - Key: _Gender_
       - Values: _1^Male,2^Female_
   2. Date of Birth
       - Field Type: DateOfBirth
       - Key: _Date_
6. We'll come back and edit the Workflows section, but for now, Save it!

#### 2. Create / Verify the Connection Opportunities
1. Create one opportunity per ministry area you have. For example, Worship, Kids Ministry, etc. We'll come back and add an attribute to each opportunity type, but for now we just need them loaded.

***

### System Communications
If you're familiar with Rock's _Email Send (System Communication)_ action at all, you know it doesn't allow a System Communication template to be selected via an attribute, but rather via a dropdown menu. With dozens of Areas of Interest, having to manually have an action _per area of interest_ just didn't scale. So to workaround that limitation, I'm pulling the HTML from the system communication and putting it into an _Email Send_ action. No other lava will get processed at send time.

The available variables are:
   - `[NICKNAME]`: is replaced with the recipient's NickName
   - `[RCKIPID]`: is replaced with a token for the recipient (generated with the PersonTokenCreate filter)
   - `[PERSONGUID]`: is replaced with the recipient's Person Alias Guid (primarily used to be passed into a workflow via the URL)

In the Communications folder there will soon be a couple of examples of email content used in a System Communication for an area of interest.

***

### Workflows
There will soon be all of the workflows needed in the Workflows folder above. Once added you'll see more details documented in the workflow directly, and then come back to complete the rest of the steps.

***

### Defined Types
We're going to build a Defined Type that's the map of the areas of interest.

#### Areas of Interest
1. Create a new Defined Type called _Areas of interest_
2. Add the following attributes (saving the defined type before adding the final one):
   - Name: Connection opportunity
      - Key: ConnectionOpportunity
      - Field Type: Connection opportunity
         - Connection Type: _whichever type was created / verified in the earlier step_
      - Require a value: _checked_
      - Show in Grid: _checked_
   - Name: Default Connector
      - Key: DefaultConnector
      - Field Type: Group Member
      - Connection Type: _select whatever group contains all possible connectors_
      - Require a value: _checked_
      - Show in Grid: _checked_
   - Name: Email Template
      - Key: EmailTemplate
      - Field Type: System Communication
      - Require a value: _unchecked_
      - Show in Grid: _checked_
   - Name: Serving Team
      - Key: ServingTeam
      - Field Type: Group
      - Require a value: _unchecked_
      - Show in Grid: _checked_
   - Name: Area of Interest When Requirements Not Met
      - Key: AreaOfInterestWhenRequirementsNotMet
      - Field Type: Defined Value
         - Defined Type: _Areas of Interest_ (yes, it references itself :))
      - Require a value: _unchecked_
      - Show in Grid: _checked_
<!--
 #### Local Mission Partners




***
-->
### Connection Opportunity Attributes
Each Connection Opportunity will need an attribute added to it.

- Name: Area of Interest
   - Key: AreaOfInterest
   - Field Type: Single-Select
   - Require a value: _checked_
   - Show in Grid: _checked_
   - Values:
   ```
     SELECT dv.[Value] AS [Text], dv.Id AS [Value]
       FROM DefinedValue dv
     	JOIN AttributeValue avco ON (avco.EntityId = dv.Id AND avco.AttributeId = 14450)
     	JOIN ConnectionOpportunity co ON (CONVERT(NVARCHAR(50),co.[Guid]) = avco.[Value])
      WHERE dv.DefinedTypeId = 130 -- area of interest defined type id
        AND co.Id = 5 -- change per opportunity to opportunity id
      ORDER BY dv.[Order] ASC
   ```
***

### Triggers
Now that the workflows are imported, we can setup the different triggers that are needed to make all of this work together.
<!--
#### Triggers
1. Trigger on Connection Request Created
    1. Go to the settings of the Online Baptism Class connection opportunity
    2. Expand Workflows and add a new Workflow Type
    3. Choose _Baptism Class - Email requestor when connection is created_ and trigger it on _Request Started_
2. Trigger on Connect Request Future Follow-Up Reached
    1. Go to the settings of the Online Baptism Class connection opportunity
    2. Expand Workflows and add a new Workflow Type
    3. Choose _Send Final Baptism Class Email if "Not Ready"_ and trigger it on _Future Followup Date Reached_
-->
<!--
#### Jobs

***
-->
### External Connection Signup
This is likely a page already configured on your external site, typically available at `/Serve`.
<!--
***

<>### Connection Opportunities/Type

***

<>### Wrapping it Up
-->
