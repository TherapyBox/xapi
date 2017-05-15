# Generic logged_out statement template

## Purpose
This activity records a user logging out of a VLE.

Examples:

- [VLE Logout](/recipes/vle/login.md#actor)

### Actor
Common entity identifier:  [ActorA](/common_structures.md#actora) page.

The actor entity describes the individual logging out of the system.

####  Entity properties:

<table>
<tr><th>Property</th><th>Description</th></tr>
<tr>
<td>actor.objectType [1]</td><td>Must have the value "Agent". Actors of type "Group" are not supported in the Jisc profile.</td>
</tr>
<tr>
<td>actor.name [0..1]</td><td>Full name of user.</td>
</tr>
<tr>
<td>	
actor.account [1] <br/>
actor.account.name [1] <br/>
actor.account.homepage [1] <br/>
</td>
<td>A JSON Object with <b>account.name</b> giving the login id for the subject of the statement and <b>account.homepage</b> giving the URL of the home page of the application for which the login id applies.</td></tr>
</table>

#### Example

``` Javascript
{

    "actor": {
        "objectType": "Agent",
        "name": "John Smith",
        "account": {
            "name": "jsmith12",
            "homePage": "https://courses.alpha.jisc.ac.uk/moodle"
        }
    },
```

### Verb
Common entity identifier: [VerbA](/common_structures.md#verba).

This statement uses the [logged out](/vocabulary.md#verbs) describes the action of logging out of a platform.


####  Entity properties:


<table>
	<tr><th>Property</th><th>Description</th></tr>
	<tr>
		<td>verb.id [1]</td>
		<td>An IRI that identifies the Verb. Valid IRIs depend on the recipe and are given on the relevant recipe page.</td>
	</tr>
	<tr>
		<td>verb.display [1]</td>
		<td>A human readable representation of Verb. It takes a RFC 5646 Language Tag.  
		The verb.display is "recommended" in the xAPI v1.0.1 specification but is not likely to be of use within the Jisc Learning Analytics architecture.</td>
	</tr>
</table>


#### Example:

``` javascript
"verb": {
        "id": "https://brindlewaye.com/xAPITerms/verbs/loggedout",
        "display": {
            "en": "logged out of"
        }
    },
``` 
### Context
Common entity identifier: [ContextA](/common_structures.md#contexta)

Since the logged-out action is not undertaken in the context of a learning activity or course, only the core attributes of context are applicable.

#### Entity properties:


<table>
<tr><th>Property</th><th>Description</th></tr>
	<tr><td>context.platform [1]</td>
	<td>The platform used in the experience of this learning activity. The value used should not change between platform upgrades and version changes and should typically be a concise name by which the application is commonly known, for example "Moodle" or "Blackboard"</td></tr>
	<tr><td>context.extensions.version [0..1]
		 context.extension.sessionId [0..1]
		 context.extension.ip-address [1]
		 </td>
		<td>Four extensions are provided for, with IRIs as defined on the <a href="vocabulary.md#41-context-extensions">vocabularies page</a>.
  	  The <b>sessionID</b> extension is the VLE session ID, or a suitably hashed version of it. A value should be provided if this information is available.<br/>
    The <b>ip-address</b> is used to identify the client's IP address. An IPv4 address is recommended.<br/>
    The <b>version</b> extension is recommended, and identifies the version of the Jisc xAPI profile found on the ReadMe page. <br/>
	The <b>courseArea</b> identifies Umbrella course/parent area by its home page URI. More information can be found on the <a href="vocabulary.md#umbrella-course-area">vocabularies page</a>.
		</td></tr></table>

### Example:

``` javascript
"context": {
        "platform": "Moodle",
        "extensions": {
 			"http://xapi.jisc.ac.uk/sessionId":"32456891",
        	"http://id.tincanapi.com/extension/ip-address": "10.3.3.48",
			"http://xapi.jisc.ac.uk/version" : "1.0"
			}
```

### Object
Common entity identifier: [ObjectA][common structures](/common_structures.md#objecta)

When the object.definition.type is "http://activitystrea.ms/schema/1.0/application".  The subType (http://xapi.jisc.ac.uk/subType) extension should be used to identify the subType of the application that is being logged into, in this example a Virtual Learning Environment (aka Learning Management System), identified by  http://id.tincanapi.com/activitytype/lms. Different application types should use the relevant subType, as defined on the [vocabularies page](/vocabulary.md#32-object-definition-extensions) page.

#### Entity properties:


<table>
	<tr><th>Property</th><th>Description</th></tr>
	<tr>
		<td>object.objectType [1]</td>
		<td>The value must be "Activity".</td>
	</tr>
	<tr>
		<td>object.id [1]</td>
		<td>An identifier for the object of the xAPI statement. This must be unique (within a given platform) across all object types.</td>
	</tr>
		<tr>
		<td>object.definition.type [1]<br />
	object.definition.name [0..1]<br />
	object.definition.extensions.http://xapi.jisc.ac.uk/subType [0..1]<br />
	object.definition.extensions.http://xapi.jisc.ac.uk/uddModInstanceID [0..1]</td>
		<td>A JSON object comprising both standard xAPI attributes and the Jisc profile 'subType' and 'uddModInstanceID' extensions.<br/>
    The <b>type</b> indicates the type of the object of the statement. It is required and valid values are listed on the <a href="vocabulary.md#31-activity-types">vocabulary page</a>.<br/>
    The <b>name</b> is optional.<br/>
    The <b>subType</b> extension may be used to indicate the sub-type of this activity, if applicable for the recipe being used to create the statement. This qualifies the object.objectType, and is described on the [vocabularies](vocabulary.md#32-object-definition-extensions) page.<br />
    The <b>uddModInstanceID</b> extension records the module instance with which the learning activity is associated. It is not usually required and overlaps with context.extension.courseArea. [See issue 140](https://github.com/jiscdev/xapi/issues/140) </td>
	</tr>
	
</table>

### Example
``` javascript

"object": {
        "objectType": "Activity",
        "id": "https://courses.alpha.jisc.ac.uk/moodle",
        "definition": {
            "type": "http://activitystrea.ms/schema/1.0/application",
            "name": {
                "en": "A Moodle course"
            },
            "extensions": {
                "http://xapi.jisc.ac.uk/subType": "http://id.tincanapi.com/activitytype/lms"
                }
            }
        },
```