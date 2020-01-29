# ajaikumar.github.io

# SOLID Principles
 It represents five principles of object-oriented programming: 
Single responsibility, Open/Closed, Liskov Substitution, Interface Segregation, and Dependency Inversion.

By these principles we can solve the main problems of a bad architecture:
• Fragility: A change may break unexpected parts—it is very difficult to detect if you don’t have good test coverage.
• Immobility: A component is difficult to reuse in another project—or multiple places of the same project—because it has too many coupled dependencies.
• Rigidity: A change requires a lot of effort because affects several parts of the project.

## The Single Responsibility Principle (SRP)
### Means: Every class we create or change should have only one responsibility

### Example (By Ajaikumar) : 

    class  CustomDataController{

     func  getAllConversations(){
           
         let  jsonData =  requestDataFromAPl()
         let  conversations =  parseAndCreateConversationsFrom(data:jsonData)
         saveToDatabase(conversations:  conversations)

         }

         private  func  requestDataFromAPI(){
        //  sends  API  Request  to  get  conversations  json  data
         }

         private  func  parseAndCreateConversationsFrom(data:Any){
        //  Parse  the  data  and  create  an array  of  Conversation  objects
         }

         private  func  saveToDatabase(conversations:[Any]){
        //  save  conversations  to  CoreDataController
         }
      
In the above example, a class is having different responsibilities and it is breaking the Single Responsibility Principle. We can achieve this by the following code.

      class  CustomDataController{
    
       let  netCore = NetAPICore()
       let  conversationFactory = ConversationFactory()
       let  coreDataController = CoreDataController()

         func  getAllConversations(){
         let  jsonData = netCore.requestDataFromAPl()
         let  conversations =     conversationFactory.parseAndCreateConversationsFrom(data:jsonData)
         coreDataController.saveToDatabase(conversations:  conversations)
        }
      }

    class  NetAPICore{
          func  requestDataFromAPI(){
        //  sends  API  Request  to get  conversations  json  
      }
     }

    class  ConversationFactory{
    func parseAndCreateConversationsFrom(data:Any)   {
        //  Parse  the  data  and  create an array of Conversation  objects
       }
    }

    class  CoreDataController{
    func  saveToDatabase(conversations: [Any]){
            //  save  conversations  to CoreDataController
      }
   }

This principle helps you to keep your classes as clean as possible, and we have the advantage of testing each and every API separately.

## Open Closed Principle
### Means: “Classes and Modules should be open for extension but closed for modification”

To understand this principle easily let's take a basic example of calculating the area of a geometry shape.

    Class Rectangle{

          let rWidth = 0
          let rHeight = 0

       init(widthParam: Double, heightParam: Double){
            rWidth =  widthParam  
        rHeight = heightParam
    }
    }

     Class AreaOfRechtangle{
           func calcArea(rect: Rectangle) -> Double{
        return rect.rWidth * rect.rHeight
       }
   }

Now the requirement has a new feature, that is to calculate the area of not only Rectangle even for Circle, Then we need to change our code again. So, We can overcome this by implementing Protocols.

     protocol Shape{
        func area() -> Double
     }

     class Rectangle: Shape{
         let rWidth = 0
         let rHeight = 0

     internal func calcArea(rect: Rectangle) -> Double{
        return “Return Rectangle area calculation here”
       }
     }

    class Circle: Shape{
        let rWidth = 0
        let rHeight = 0

      internal func calcArea(rect: Rectangle) -> Double{
        return “Return Circle area calculation here”
      }
     }

    class AreaCalc{
      func area(shape: Shape) -> Double{
        return shape.area()
      }
      } 
 
If there is a requirement tomorrow to calculate the area of even Triangle, we should be able to achieve it without modifying the existing AreaCalculator class.

## Liskov’s Substitution Principle

Child classes should never break the parent class type definitions

It means, we must make sure that new derived classes should extend the base classes without changing the base 
 class behavior

    Protocol Shape{
              func area() -> Double
    }

    Class Rectangle: Shape{
            let rWidth = 0
            let rHeight = 0

        internal func calcArea(rect: Rectangle) -> Double{
        return “Return Rectangle area calculation here”
        }
    }

    class Circle: Shape{
            let rWidth = 0
            let rHeight = 0

        internal func calcArea(rect: Rectangle) -> Double{
        return “Return Circle area calculation here”
        }
    }

The Interface Segregation Principle

Clients should not be forced to depend upon interfaces that they do not use.

    protocol GestureProtocol {
        func didTap()
        func didDoubleTap()
        func didLongPress()
    }

The Protocol implementation as follows

     class SuperButton: GestureProtocol {
            func didTap() {
        // send tap action
        }

        func didDoubleTap() {
        // send double tap action
        }

        func didLongPress() {
           // send long press action
        }
    }

In some cases, if we need only one function implementation and not required of remaining functions even those cases we need to implement all functions to satisfy the protocol implementation. To overcome this we can follow the ISP.

    protocol TapProtocol {
             func didTap()
    }

    protocol DoubleTapProtocol {
            func didDoubleTap()
    }

    protocol LongPressProtocol {
            func didLongPress()
    }

    class PoorButton: TapProtocol {
            func didTap() {
                    // send tap action
                 }
    }



## The Dependency Inversion Principle (DIP)
High-level modules should not depend on low-level modules both should depend on Abstractions


       class ConversationDataController    {
           let  database:  CoreDataController
           init(inDatabase:CoreDataController){
            database =  inDatabase
           }

          func  getAllConversations(){
               let  conversations =  [Any]()     
                //  array  of  previous  Conversations
                //array  of  previous  conversation  is  downloaded  from API,  parsed  and created. database. 
               saveToDatabase(conversations:  conversations)
          }
      }
       class CoreDataController     {
        func  saveToDatabase(conversations:[Any]){
        //  save  conversations  to CoreDataController 
        }
    }

here the CoreDataController is a low-level module, its easy to reuse in other projects, the problem is with high-level module ConversationDataController, it's tightly coupled with CoreDataController.

We can solve this dependency using a Database Protocol. In this way, ConversationDataController can use the abstract protocol without caring for the type of database used, let's see how the ConversationDataController will look after applying this.


    protocol  Database{
        func  saveToDatabase(conversations:[Any])
    }

    class ConversationDataController{

        let  database: Database

        init(inDatabase:Database){

                database=  inDatabase
        }

        func  getAllConversations(){
            
            let  conversations = [Any]()   //  array of  previous  Conversations
            //array  of  previous  conversation  is downloaded  from API,  parsed  and  created.
            
            database.saveToDatabase(conversations:  conversations)
        }
    }
        
    class CoreDataController : Database{

        func  saveToDatabase(conversations:[Any]){
                //  save  conversations  to CoreDataController
        }
        }


Hope you understand all the principles well. 

Thank you.
Ajaikumar.



