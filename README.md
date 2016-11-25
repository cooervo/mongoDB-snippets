
#Snippets

Given a collection of Projects with the following structure:



    {
        "_id" : ObjectId("57f59005c451fb467"),
        "projectStatus" : "On-Hold",
        "createdAt" : ISODate("2016-10-05T18:00:31.000Z"),
        "createdBy" : "68SmxVkqiVgPzzzdW",
        "name" : "Example Name",
        "utilityName" : "PGE",
        "state" : "OR",
        "county" : "Clackamas",
        "allocInvestor" : "Investor X",
        "projectManager" : "zNJ78fODrFLIyWGAr",
        "developer" : "CCR",
        "projectID" : "OR-PGE-020",
        "location" : {
            "gps" : {
                "gpsLatitude" : "45.17226",
                "gpsLongitude" : "122.41443"
            }
        },
        
    }


----------


### Field Exists

Search if field `zoning.zonMtg.meetings.tasks` exist and only show (SELECT) `name`& `zoning.zonMtg.meetings.tasks` fields to show:

	var localOptions = { name: 1, "zoning.zonMtg.meetings.tasks":1 }
	var filter = {'zoning.zonMtg.meetings.tasks':{$exists:true}};   
	db.projects.find(filter, localOptions);
	

Search if field `nameCap` exists:

	var filter = {'nameCap':{$exists:true}};   
	db.getCollection('equipment').find(filter); //Search in collection equipment
	db.getCollection('projects').find(filter); //Search in collection projects

#### Output:
Brings back the object with existing field

Search if field `utility.ica.icVoltage` exists:

	var filter = {'utility.ica.icVoltage':{$exists:true}};
	db.getCollection('projects').find(filter, {'utility.ica.icVoltage':1})


----------

### Field equals

db.getCollection('projects').find({allocInvestor: "Query X"});

#### Output:

Brings back the objects where allocInvestor = "Query X"

----------


### Aggregation count

On collection projects, group by allocInvestor, where allocInvestor is not null and state equals NY

	db.getCollection('projects').aggregate( //On collection projects
    {
        $match: {
            allocInvestor: {$ne: null}, //where allocInvestor is not null and... 
            state: "NY" // state = NY
        }
    },
    {
        $group: { //Group by...
            _id: "$allocInvestor", //...allocInvestor 
            count: {$sum: 1} //...and bring back a variable of the count by each allocInvestor
        }
    }
 );

#### Output:
	
	/* 1 */
	{
	    "_id" : "NY CDG Tranche 1",
	    "count" : 6.0
	}
	
	/* 2 */
	{
	    "_id" : "NY CDG Tranche 2",
	    "count" : 11.0
	}
	
	/* 3 */
	{
	    "_id" : "NY NRG Sale",
	    "count" : 5.0
	}
	
	/* 4 */
	{
	    "_id" : "NY CDG Tranche 3",
	    "count" : 16.0
	}
