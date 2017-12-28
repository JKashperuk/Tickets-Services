First of all you must go to http://www.mockaroo.com/ and register there. Then you create new schema. The first schema is 'route_template'. There are such filds, its types and options:

| Field Name  |       Type      |            Options            |
|-------------|-----------------|-------------------------------|
| _id	        | MongoDB ObjectID| none	                        |
| route_key   | Char Sequence 	| ###^	                        |
| duration    | Char Sequence   | 00:0#:##:00                   |
| __city1     | City            | none                          |
| __city2     | City            | none                          |
| __city3     | City            | none                          |
| __city4     | City            | none                          |
| __city5     | City            | none        		        |
| stations    | Formula 	      | [ __city1,__city2,...,__city5]|
| created_date| Date	           | none                          |
| updated_date| Date            | none                          |


After that you can choose amount of rows and set JSON format. Then you need to click on the button 'Download Data'.
Further you must create new java project and copy json-file to project folder. Then you need to run following script:
```
	 BufferedReader br = new BufferedReader(new FileReader("route_template.json"));
		 String dbName = "route_template";
		 try {
		 StringBuilder sb = new StringBuilder();
		 String line = br.readLine();
		
		 while (line != null) {
		 sb.append(line);
		 sb.append(System.lineSeparator());
		 line = br.readLine();
		 }
		 String everything = sb.toString();
		 String update = everything.replace("{\"$oid\":", "ObjectId(");
		 String update1 = update.replace("{\"$date\":", "new Date(");
		 String update2 = update1.replace("}}", ")}");
		 String update3 = update2.replace("route_id\":", "route_id\":ObjectId(");
		 String update4 = update3.replace("\",\"passport", "\"),\"passport");
		 String update5 = update4.replace("\"new Date(", "new Date(\"");
		 String update6 = update5.replace(")\"", "\")");
		 String update7 = update6.replace("\",\"departure_city\":","\"),\"departure_city\":");
		 String update8 = update7.replace("departure_date\":", "departure_date\":new Date(");
		 String update9 = update8.replace("},\"route_key", "),\"route_key");
		 String update10 = update9.replace("},\"updated_date", "),\"updated_date");
		 StringBuilder startUp = new StringBuilder(update10);
		 startUp.insert(0,"db."+dbName+".insertMany(");
		 startUp.insert(startUp.length(), ");");
		
		 System.out.println(startUp);
	} finally {
		br.close();
	}
}
```
This script changes source json-text to the format that you need. After this action you need to create js-file, enter the command 'db = db.getSiblingDB('ticket_db')' and copy text from eclipse console to this file.

The next step is adding 'route_template' schema to the datasets in http://www.mockaroo.com/. For this step you need to click on 'DATASETS' in menu. Than click on 'Upload a New Dataset', enter a name and choose the file. But the file must be .csv. You can convert json to csv by online converter, for example, https://json-csv.com/, https://konklone.io/json/ etc.

The next schema is 'route'. There are such filds, its types and options:

| Field Name 	          | Type 	      | Options 						|
|-----------------------|-------------|---------------------|
| _id	     	            | MongoDB ObjectID   | none							                                |
| route_template_id   	| Dataset Column     | address, _id, random		                          |
| transport_id 		      | Dataset Column     | transport, __id, random					                |
| departure_date 	      | Date	             | 12/26/2016 to 06/26/2017 in mongoDB ISO 	      	|
| created_date 		      | Date	             |	12/26/2016 to 06/26/2017 in mongoDB ISO	        |
| updated_date          | Date               |	12/26/2016 to 06/26/2017 in mongoDB ISO	       	|


After that you can choose amount of rows and set JSON format. Then you need to click on the button 'Download Data'.
Further you must copy json-file to java project folder. Then you need to run following script:
```
		 BufferedReader br = new BufferedReader(new FileReader("route.json"));
		 String dbName = "route";
		 try {
		 StringBuilder sb = new StringBuilder();
		 String line = br.readLine();
		
		 while (line != null) {
		 sb.append(line);
		 sb.append(System.lineSeparator());
		
		 line = br.readLine();
		 }
		 String everything = sb.toString();
		 String update = everything.replace("{\"$oid\":", "ObjectId(");
		 String update2 = update.replace("{\"$date\":", "new Date(");
		 String update3 = update2.replace("\"new Date(", "new Date(\"");
		 String update4 = update3.replace("},\"created_date", "),\"created_date");
		 String update5 = update4.replace("},\"updated_date", "),\"updated_date");
		 String update6 = update5.replace("}}", ")}");
		 String update7 = update6.replace("},\"route_template_id\":", "),\"route_template_id\":ObjectId(");
		 String update8 = update7.replace(",\"transport_id\":", "),\"transport_id\":ObjectId(");
		 String update9 = update8.replace(",\"departure_date", "),\"departure_date");
		 
		 StringBuilder startUp = new StringBuilder(update9);
		 startUp.insert(0,"db."+dbName+".insertMany(");
		 startUp.insert(startUp.length(), ");");
	} finally {
		br.close();
	}
}
```
After this action you need to copy text from eclipse console to js-file that you created.

The next step is adding 'route' schema to datasets in http://www.mockaroo.com/. This step described above.

The next schema is 'transport'. There are such filds, its types and options:

| Field Name 	         | Type 	           | Options 						    |
|----------------------|-------------------|------------------------|
| _id	     	           | MongoDB ObjectID  | none						                                       |
| __count1             | Number            | min - 10, max - 100              		                 |
| __count2             | Number	           | min - 10, max - 100              		                 |
| __count3		         | Number   	       | min - 10, max - 100         			                     |
| __count4             | Number	           | min - 10, max - 100				                           |
| __type_of_place1     | Char Sequence     | REGULAR          				                             |
| __type_of_place2 	   | Char Sequence     | ECONOMY					    	                               |
| __type_of_place3 	   | Char Sequence     | PREMIUM_ECONOMY				 	                             |
| __type_of_place4	   | Char Sequence     | BUSINESS					 	                                   |
| available_places     | Formula           | [{type_of_place:__type_of_place1, count:__count1},{type_of_place:__type_of_place2, count:__count2},{type_of_place:__type_of_place3, count:__count3},{type_of_place:__type_of_place4, count:__count4}]                                                     |


After that you can choose amount of rows and set JSON format. Then you need to click on the button 'Download Data'.
Further you must copy json-file to java project folder. Then you need to run following script:
```
		 BufferedReader br = new BufferedReader(new
		 FileReader("transport.json"));
		 String dockName = "transport";
		 try {
		 StringBuilder sb = new StringBuilder();
		 String line = br.readLine();
		
		 while (line != null) {
		 sb.append(line);
		 sb.append(System.lineSeparator());
		
		 line = br.readLine();
		 }
		 String everything = sb.toString();
		 //System.out.println(everything);
		 String update = everything.replace("{\"$oid\":", "ObjectId(");
		 String update2 = update.replace("{\"$date\":", "new Date(");
		 String update3 = update2.replace("\"new Date(", "new Date(\"");
		 String update4 = update3.replace("},\"created_date", "),\"created_date");
		 String update5 = update4.replace("},\"updated_date", "),\"updated_date");
		 String update6 = update5.replace("}}", ")}");
		 String update7 = update6.replace("},\"available_places","),\"available_places");
		
		 StringBuilder startUp = new StringBuilder(update7);
		 startUp.insert(0,"db."+dockName+".insertMany(");
		 startUp.insert(startUp.length(), ");");
	} finally {
		br.close();
	}
}
```
After this action you need to copy text from eclipse console to js-file that you created.

The next step is adding 'transport' schema to the datasets in http://www.mockaroo.com/. This step described above.

The next schema is 'ticket'. There are such filds, its types and options:

| Field Name     | Type 	          | Options 				       |
|----------------|------------------|------------------------|
| _id	           | MongoDB ObjectID | none				      |
| route_id       | Dataset Column   | hotel, _id$oid, random		              |
| user_id        | MongoDB ObjectID | none				                            |
| passport_date  | Catch Phrase     | ^^#####			                            |
| departure_date | Dataset Column   | route_, departure_date		              |
| departure_city | Dataset Column   | route_template, stations/0              |
| arrival_city   | Dataset Column   | route_template, stations/4              |
| created_date   | Date	   	        | 12/26/2016 to 06/26/2017 in mongoDB ISO |
| updated_date   | Date	            | 06/26/2017 to 12/26/2017 in mongoDB ISO |


After that you can choose amount of rows and set JSON format. Then you need to click on the button 'Download Data'.
Further you must copy json-file to java project folder. Then you need to run following script:
```
    BufferedReader br = new BufferedReader(new FileReader("ticket.json"));
    String dockName = "ticket";
    try {
    StringBuilder sb = new StringBuilder();
    String line = br.readLine();
    while (line != null) {
    sb.append(line);
    sb.append(System.lineSeparator());
    line = br.readLine();
		 }
		String everything = sb.toString();
		String update = everything.replace("{\"$oid\":", "ObjectId(");
		String update2 = update.replace("{\"$date\":", "new Date(");
		String update3 = update2.replace("\"new Date(", "new Date(\"");
		String update4 = update3.replace("},\"created_date", "),\"created_date");
		String update5 = update4.replace("},\"updated_date", "),\"updated_date");
		String update6 = update5.replace("}}", ")}");
		String update7 = update6.replace("},\"route_id\":", "),\"route_id\":ObjectId(");
		String update8 = update7.replace(",\"departure_city", "),\"departure_city");
		String update9 = update8.replace(",\"departure_date\":", ",\"departure_date\": new Date(");
	  String update10 = update9.replace("},\"passport_data", "),\"passport_data");
		String update11 = update10.replace(",\"user_id","),\"user_id");
				 
		StringBuilder startUp = new StringBuilder(update11);
		startUp.insert(0,"db."+dockName+".insertMany(");
		startUp.insert(startUp.length(), ");");
		System.out.println(startUp);
	} finally {
		br.close();
	}
}
```
After this action you need to copy text from eclipse console to js-file that you created.

The next schema is 'trip'. There are such filds, its types and options:

| Field Name         | Type 	          | Options 				                   | 
|--------------------|------------------|---------------------------------   |
| _id	               | MongoDB ObjectID | none			            	           |
| route_template_ id | Dataset Column   | route_template, _id$oid, random	   |
| arrival_city 	     | Dataset Column	  | route_template, stations/4         |
| departure_city     | Dataset Column   | route_template, stations/0		     |
| arrival_time       | Char Sequence    | new Date(2017-12-27T0#:0#:00.000Z) |
| departure_time     | Char Sequence    | new Date(2017-12-27T1#:0#:00.000Z) |
| duration           | Char Sequence    | 00:0#:0#:00                        |
| price              | Char Sequence    | min - 150, max - 12000                   |
| created_date       | Date	          	| 12/26/2016 to 06/26/2017 in mongoDB ISO     | 
| updated_date       | Date	            | 06/26/2017 to 12/26/2017 in mongoDB ISO     |


After that you can choose amount of rows and set JSON format. Then you need to click on the button 'Download Data'.
Further you must copy json-file to java project folder. Then you need to run following script:
```
 		BufferedReader br = new BufferedReader(new FileReader("trip.json"));
		String dockName = "trip";
		try {
		StringBuilder sb = new StringBuilder();
		String line = br.readLine();

		while (line != null) {
		sb.append(line);
		sb.append(System.lineSeparator());

		line = br.readLine();
		}
		String everything = sb.toString();
	
		String update = everything.replace("{\"$oid\":", "ObjectId(");
		String update2 = update.replace("{\"$date\":", "new Date(");
		String update3 = update2.replace("\"new Date(", "new Date(\"");
		String update4 = update3.replace("},\"created_date", "),\"created_date");
		String update5 = update4.replace("},\"updated_date", "),\"updated_date");
		String update6 = update5.replace(")\",\"d", "\"),\"d");
		String update7 = update6.replace("}}", ")}");
		String update8 = update7.replace("},\"route_template_id\":", "),\"route_template_id\":ObjectId(");
		String update9 = update8.replace(",\"arrival_city","),\"arrival_city");
			
			StringBuilder startUp = new StringBuilder(update9);
			startUp.insert(0, "db." + dockName + ".insertMany(");
			startUp.insert(startUp.length(), ");");
			System.out.println(startUp);
	} finally {
		br.close();
	}
}
```
After this action you need to copy text from eclipse console to js-file that you created.

After that you need to start mongodb, mongoshell, enter the command 'load("<path_to_script/<file_name>.js>")', press 'Enter' and you will get a database named hotel_db with filled documents.
