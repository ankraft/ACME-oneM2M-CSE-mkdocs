# Resource Type Policies

This article describes the policies that are defined for resource types in the ACME CSE. 

Instead of defining the atrributes and child resource types for each resource type in the code, they are defined in an external JSON file that is loaded during CSE initialization. This allows to easily change the resource type definitions without changing the code. All the definitions are in one place and can be easily accessed and modified.

Resource type policy files have the extension `.rtp` and are automatically imported only[^1] from the [init](https://github.com/ankraft/ACME-oneM2M-CSE/blob/master/acme/init){target=_new} directory.

[^1]: Different from flex container policies, resource type policies are only imported from the primary init directory, not from the secondary init directory. This is because resource type policies are fundamental for the CSE and should not be changed after the CSE has been started. 

## File Format

The resource type policy files are JSON files (with comments). The format and the elements are described in the following example. 

```json title="resourceTypes.rtp"
{
	// The short name of the resource type, e.g. "CNT" for container
	"resourceTypeName": {
		
		// The resource type name including its namespace, e.g. "m2m:cnt" for container
		"typeName": <string namespace:shortname>,

		// The resource type's full name.
		"fullName": <string fullName>,

		// The resource type's numeric identifier.
		"type": <numeric value>,

		// This attribute must be present if the resource type is a ManagementObject
		// specialization. It is used to specify the management type of the resource type.
		// It is not present for non-management resource types.
		"mgmtType": <numeric value>,

		// The resource type's announced type. 
		// This is the resource type that is used for the announced version of the resource. 
		// It is not present when the resource type is not announcable.
		"announcedType": <string announcedResourceTypeName>,

		// This attribute must be present if the resource type is a virtual resource.
		// It specifies the value for the "resourceName" attribute for the virtual resource.
		"virtualResourceName": <string virtualResourceName>,

		// This attribute is true if the resource type is a container resource type, 
		// false otherwise.
		// The default value is false.
		"isContainer": <boolean value>,

		// This attribute is true if the resource can receive notifications, false otherwise
		// The default value is false.
		"isNotificationEntity": <boolean value>,

		// This attribute is true if the resource type is an announced resource type,
		// false otherwise.
		// The default value is false.
		"isAnnouncedResource": <boolean value>,

		// This attribute is true if the resource type is an instance resource type, e.g. 
		// for a "ContentInstance" resource type, false otherwise. 
		// The default value is false.
		"isInstanceResource": <boolean value>,

		// This attribute is true if the resource type is a specialization of the 
		// "ManagementObject" resource type, false otherwise.
		// The default value is false.
		"isMgmtSpecialization": <boolean value>,

		// This attribute is true if resources of this type can be created through a 
		// CREATE request, false otherwise.
		// The default value is true
		"isRequestCreatable": <boolean value>,

		// This attribute is true if the resource type is the base type for a specialization
		// e.g. in case of the "FlexContainer" resource type, false otherwise.
		// The default value is false.
		"isSpecializationBaseResource": <boolean value>,

		// This attribute is true if reosurce of this type do inherit access control policies
		// from their parent resource, false otherwise.
		// The default value is false.
		"inheritACP": <boolean value>,

		// This attribute is true if the resource type is not a real resource type,
		// but only used internally for the CSE, false otherwise.
		// The default value is false.
		"isInternalType": <boolean value>,

		// This attribute is a list of attributes that are defined for the resource type.
		// Each entry specifies a single attribute. 
		// The attributes are defined as strings containing the attribute shortname, 
		// e.g. "rn" for the resource name.
		"attributes": [
			<string attributeShortName>,
			...
		],

		// This attribute is a list of allowed child resource types for the resource type.
		// Each entry specifies a single child resource type by its short name, e.g.
		// "CNT" for container.
		"childResourceTypes": [
			<string childResourceTypeShortName>,
			...
		]
}

```


## Example

The following example shows the resource type policy definition for the *&lt;container>* resource type. It defines necessary properties, the attributes and the allowed child resource types for the *container* resource type.

```json title="resourceTypes.rtp"
{
	// ...

	"CNT": {
		"typeName": "m2m:cnt",
		"fullName": "Container",
		"announcedType": "CNTAnnc",
		"type": 3,
		"isContainer": true,

		"attributes": [
			// Common and universal attributes
			"rn",
			"ty",
			"ri",
			"pi",
			"ct",
			"lt",
			"et",
			"lbl",
			"cstn",
			"acpi",
			"at",
			"aa",
			"ast",
			"daci",
			"st",
			"cr",
			"loc",

			// Resource attributes
			"mni",
			"mbs",
			"mia",
			"cni",
			"cbs",
			"li",
			"or",
			"disr",
		],
		"childResourceTypes": [
			"ACTR",
			"CIN",
			"CNT", 
			"CNT_LA",
			"CNT_OL",
			"FCNT",
			"SMD",
			"SUB",
			"TS"
		]
	},

	// ...
}
```
