# MongoDB-Day2

# Pre-requisite Queries
db.attendance.find({})

db.attendance.insert({userId: 1,attendance: 32,mentorId: 1,batchId: 1,total: 34,noAttendance: 2})
db.attendance.insert({userId: 2,attendance: 18,mentorId: 2,batchId: 2,total: 32,noAttendance: 14})
db.attendance.insertMany([{userId: 3,attendance: 23,mentorId: 3,batchId: 3,total: 34,noAttendance: 11},{userId: 4,attendance: 28,mentorId: 4,batchId: 4,total: 34,noAttendance: 6},{userId: 5,attendance: 34,mentorId: 3,batchId: 3,total: 34,noAttendance: 0}])
db.attendance.remove({_id: {$in : [ObjectId("6272bedf999ec71b41effa97"),ObjectId("6272bedf999ec71b41effa98"),ObjectId("6272bedf999ec71b41effa99"),ObjectId("6272bedf999ec71b41effa9a"),ObjectId("6272bedf999ec71b41effa9b")]}})


db.codekata.find()

db.codekata.insertOne({userId: 1,solvedProblems: 230,total: 1320,lastSolved: new Date().toLocaleDateString()})

db.codekata.insertOne({userId: 2,solvedProblems: 500,total: 1320,lastSolved: new Date(2021,11,23).toLocaleDateString()})

db.codekata.insertMany([{userId: 3,solvedProblems: 270,total: 1320,lastSolved: new Date(2022,0,02).toLocaleDateString()},{userId: 4,solvedProblems: 230,total: 1320,lastSolved: new Date(2022,3,4).toLocaleDateString()}])

db.company_drives.find()

db.company_drives.insertMany([{id: 1,name: "Amazon",date: new Date(2020,12,10)},{id: 2,name: "Paypal",date: new Date(2020,01,10)}])

db.company_drives.insertMany([{id: 3,name: "Zoho",date: new Date(2020,09,16)},{id: 2,name: "ChargeBee",date: new Date(2020,9,27)}])

db.company_drives.update({_id: ObjectId("627206fd999ec71b41effa8e")}, 
    {$set:{date : new Date(2020,9,27).toLocaleDateString()}}, 
    { multi: false, upsert: false}
)

db.company_drives.remove({_id : {$in : [ObjectId("627205ff999ec71b41effa8b"),ObjectId("627205ff999ec71b41effa8c")]}})

db.topics.find()

db.topics.insertMany([{id: 1,topic: "DOM manipulation",date: new Date(2021,03,21).toLocaleDateString(),taskId: 1},{id: 2,topic: "DOM manipulation - session 2",date: new Date(2021,03,24).toLocaleDateString(),taskId: 2}])

db.topics.insertMany([{id: 3,topic: "Node",date: new Date(2021,09,14).toLocaleDateString(),taskId: 3},{id: 4,topic: "React",date: new Date(2021,09,24).toLocaleDateString(),taskId: 4}])

db.tasks.find({})

db.tasks.insertMany([{id: 1,task : "DOM Event Listener"},{id: 2,task : "DOM Bootstrap"},{id: 3,task: "creating server using http"},{id: 4,task: "redux cart"}])

db.users.find();

db.users.update({_id : ObjectId("6271fd71999ec71b41effa72")}, 
    {$set:{id : 2}}, 
    { multi: false, upsert: false}
)
db.attendance.find();

db.company_drives.find();

db.attendance.remove({_id: {$in : [ObjectId("62720315999ec71b41effa78"),ObjectId("627203a3999ec71b41effa79"),ObjectId("627203a3999ec71b41effa7a"),ObjectId("627203a3999ec71b41effa7b")]}})


// create collection
use assignment

// get all collection names
db.getCollectionNames();

// create and insert data into users table
db.users.insertOne({id: 1,name: "user 1",mentor_Id: null,attendance: 80})
db.users.insertOne({id: 3,name: "user 3",mentor_Id: 2,attendance: 70})
db.users.insertOne({id: 4,name: "user 4",mentor_Id: 2,attendance: 45})
db.users.insertOne({id: 5,name: "user 5",mentor_Id: 2,attendance: 70})



// create and insert data into mentor table
db.mentors.insertOne({id: 1,name:"Mentor 1",type: "weekend",company: "infosys",batchId: 1,batch: ["B30WE","B30WD"],expertise: ["mongo","react","JS","AWS"],joinedDate: new Date().toLocaleDateString()})

db.mentors.insertOne({id: 2,name:"Mentor 2",type: "weekday",company: "infosys",batchId: 2,batch: ["B29WD"],expertise: ["mongo","Node","HTML","DS"],joinedDate: new Date(2021,01,03).toLocaleDateString()})

db.mentors.insertMany([{id: 3,name:"Mentor 3",type: "weekday",company: "CTS",batchId: 3,batch: ["B28WD"],expertise: ["mongo","Node","CSS","System Design"],joinedDate: new Date(2021,11,23).toLocaleDateString()},{id: 4,name:"Mentor 4",type: "weekend",company: "Amazon",batchId: 4,batch: ["B28WE"],expertise: ["mongo","Node","CSS","System Design"],joinedDate: new Date(2022,02,14).toLocaleDateString()}])

db.mentors.insertOne({id: 5,name:"Mentor 5",type: "weekday",company: "Paypal",batchId: 5,batch: ["B27WD"],expertise: ["mongo","CSS","HTML","DS","Node","Angular"],joinedDate: new Date(2021,0,7).toLocaleDateString()})

// remove data from mentor table
db.mentors.remove({_id: ObjectId("6271fd71999ec71b41effa73")})

// get all documents from mentor table
db.attendance.find()
db.company_drives.updateOne({"name" : "ChargeBee" },{$set:{id: 4}})

db.users.find();
db.users.updateOne({"id" : 5},{$set: {class1 : false,task1: false,date:new Date(2020,09,15).toLocaleDateString()}},{upsert:false});


# Design database for Zen class programme
users
codekata
attendance
topics
tasks
company_drives
mentors


Find all the topics and tasks which are thought in the month of October     
db.topics.aggregate([
    {
        $lookup: {
            from: "tasks",
            localField: "taskId",
            foreignField: "id",
            as: "TaskId"
        }
    },
    { 
        $project: {
            _id: 0,
            topic : {$cond: { if: { $eq: [{$month : { $dateFromString: {
                    dateString: "$date",
                    format: "%m/%d/%Y"
                } }}, 10] }, then: '$topic',else: "$$REMOVE"}},
            task : {$cond: { if: { $eq: [{$month : { $dateFromString: {
                    dateString: "$date",
                    format: "%m/%d/%Y"
                } }}, 10] }, then: '$TaskId.task',else: "$$REMOVE"}}
        }
    }
])
<img width="922" alt="Screen Shot 2022-05-06 at 10 09 48 PM" src="https://user-images.githubusercontent.com/26063120/167175957-de2d8fe9-db01-4fb7-840d-357db3c160ee.png">      

Find all the company drives which appeared between 15 oct-2020 and 31-oct-2020      
db.company_drives.find({$and : [{date: {$gt : "10/15/2020"}},{date: {$lt : "10/31/2020"}}]})      
<img width="788" alt="Screen Shot 2022-05-06 at 10 10 02 PM" src="https://user-images.githubusercontent.com/26063120/167175910-a25c3a5a-2bd6-412c-88f7-831b2da63beb.png">      

Find all the company drives and students who are appeared for the placement.    
db.users.aggregate([
    {
      $match : {
          drive_id : {$gt : 0}
      }  
    },
    {
    $lookup : {
        from : "company_drives",
        localField: "drive_id",
        foreignField: "id",
        as: "CompanyDetails"
    }
    },
    {
    $project : {
        "id":1,
        "name": 1,
        "result" : "$CompanyDetails.name"}
        }
])

<img width="842" alt="Screen Shot 2022-05-06 at 10 34 46 PM" src="https://user-images.githubusercontent.com/26063120/167179225-bb4d161c-c15d-439c-bf00-14200c251dde.png">     

Find the number of problems solved by the user in codekata     
db.users.aggregate([{
    $lookup: {
       from: "codekata",
       localField: "id",
       foreignField: "userId",
       as: "UserId"
    }},
    {
    $project: {
        "UserId.solvedProblems": 1,
        "_id": 0,
        "name":1
    }},
])      
<img width="774" alt="Screen Shot 2022-05-06 at 10 10 32 PM" src="https://user-images.githubusercontent.com/26063120/167175845-cd33423c-e962-4893-824d-5f51134faa2f.png">     

Find all the mentors with who has the mentee's count more than 15       
db.users.aggregate([
    {
        $group: {
            _id: "$mentor_Id",
            count: {"$sum" : 1}
        }
    },
    {
    $lookup : {
        from : "mentors",
        localField: "_id",
        foreignField: "id",
        as : "mentor_details"
    }
    },
    {
        $project: {
            count : "$count",
            name: "$mentor_details.name",
            _id : 0
        }
    },{
        $match:{ "count" : {$lt : 15}}
        }
])      
<img width="852" alt="Screen Shot 2022-05-06 at 10 10 41 PM" src="https://user-images.githubusercontent.com/26063120/167175811-3ab6ea56-3167-4e9c-89b1-b845a7ed4c97.png">    


Find the number of users who are absent and task is not submitted  between 15 oct-2020 and 31-oct-2020      
db.users.find({$and :[{class1 : {$in : [false]}},{$or : [{date : {$lt : "10/15/2020"}},{date : {$gt : "10/31/2020"}}]}]})     
<img width="996" alt="Screen Shot 2022-05-06 at 10 10 11 PM" src="https://user-images.githubusercontent.com/26063120/167175882-7e8d80a5-9c15-461a-bfa7-66ff853a7073.png"> 

