package routerrental_package;
import java.io.PrintStream;
import java.util.Scanner;
import java.util.*;

/** Main Class */
public class RouterRental {
     /**
     * @param args the command line arguments
     * static method
     */
    public static void main(String[] args) 
    {
        // TODO code application logic here
        CustomerBase customer[] = new CustomerBase[3];
        customer[0] = new resident("Ahmed");
        customer[1] = new foreigner("Jack");
        customer[2] = new foreigner("Natalia");
        
        ArrayList<Router> routers;
        routers = new ArrayList<>();
        Router router;
        
        router = new Router("Huawei310", 4);
        routers.add(router);
        router = new Router("Huawei330", 5);
        routers.add(router);
        router = new Router("Huawei350", 6);
        routers.add(router);
        
        System.out.println("Router Models:");
        routers.forEach((router1) -> {
            router1.display();
        });
        
        customer[0].rent(routers.get(0),"daily",9,6,2020,6);
        
                
        customer[0].rent(routers.get(1),"daily", 9, 6, 2020, 10);
        customer[0].invoice.calculateFees();
        
        customer[1].rent(routers.get(0),"monthly", 15, 6, 2020, 2);
        customer[1].invoice.calculateFees();
        customer[1].rentCancelling(customer[1].reservation);
        
        customer[1].rent(routers.get(2),"daily", 10, 6, 2020, 5);
        customer[1].invoice.calculateFees();
        customer[1].extendDuration(2, 4, "daily");
        customer[1].invoice.calculateExtendedFees();
        
        customer[2].rent(routers.get(2),"monthly", 15, 6, 2020, 10);
        customer[2].changeRouterModel(5, routers.get(0));
        customer[2].invoice.calculateFees();
        
        customer[1].reportIssue("Internet speed is slow");
        
    }
    
}






/**
 * interface of customer
 * its have a set of functions we must implement it in any child
 */
interface Customer{ 
    /**Rent Method used to make the customer rent a router
     *@param router give it the router details
     *@param type give it the type of renting{daily, monthly, weekly}
     *@param day give it the starting day 
     *@param month give it the starting month
     *@param year give it the starting year
     */
    public abstract void rent(Router router,String type, int day, int month, int year, int number);
    /**A method is used to cancel a router renting
     *@param cancelledReservation give it the Reservation that I want to cancel
     */
    public void rentCancelling(Reservation cancelledReservation); 
    /**A method used to extend the duration of renting
     *@param newDuration give it the number of days/ months/ weeks that I want to add
     *@param reservationID give it the reservation ID that I want to extend its duration
     *@param type give it the type of extension daily, weekly, monthly 
     */
    public void extendDuration(int newDuration, int reservationID, String type );
    /**A method used to change the router model that I reserved before
     *@param reservationNumber give it the reservation number that I want to change the router model
     *@param router give it the new router details
     */
    public void changeRouterModel(int reservationNumber, Router router);
    /**A method used to report an issue*/
    public void reportIssue(String message); 
}

/** an implementation of Customer interface*/
abstract class CustomerBase implements Customer{
     Reservation reservation;
    Invoice invoice;
    ArrayList<Invoice> invoices;
    /**Customer Name*/
    public String customerName;
    String reportIsuue;
    Date current = new Date();
    /**The resident class constructor
     *@param name give the resident a name
     */
     public CustomerBase(String name) {
        this.customerName = name;
        reportIsuue = "No Issue is found";
        customerName = name;
        invoices = new ArrayList<Invoice>();
    }
        @Override //overriding
        /**Rent Method used to make the resident rent a router
         *@param router give it the router details
         *@param type give it the type of renting{daily, monthly, weekly}
         *@param day give it the starting day 
         *@param month give it the starting month
         *@param year give it the starting year
         */    
       public abstract void rent(Router router,String type,int startday, int startmonth, int startyear, int number);
        @Override //overriding
        /**A method is used to cancel a router renting
        *@param cancelledReservation give it the Reservation that I want to cancel
        */
        public void rentCancelling(Reservation cancelledReservation) {
        boolean error = false;
        try {
            if(reservation.getStartDate().year == current.getYear())
            {
                if(reservation.getStartDate().month == current.getMonth())
                {
                    if((reservation.getStartDate().day - current.getDate()) <=1)
                    {
                        error = true;
                        throw new excep("You can't cancel the router rent if the start day after the current day with 2 days");
                    }
                }
            }
            
        } catch (excep ex) {
            System.err.println("Error"+ex.getMessage());
        }
        if(!error)
        {
            for(Invoice invoice : invoices)
                if(invoice.reservation == cancelledReservation)
                {
                    invoices.remove(invoice);
                    break;
                }
            
            System.out.println("Reservation Number #" + reservation.reservationID + " has been cancelled successfully");
        }
    }
        @Override //overriding
        /**A method used to extend the duration of renting
        *@param newDuration give it the number of days/ months/ weeks that I want to add
        *@param reservationID give it the reservation ID that I want to extend its duration
         *@param type give it the type of extension daily, weekly, monthly 
         */
        public void extendDuration(int newDuration, int reservationID, String type ){
        for(Invoice invoice : invoices)
        {
            if(invoice.reservation.reservationID == reservationID)
            {
                invoice.reservation.extendedDueDate(type, newDuration);
            }
        }
        System.out.println("\nDuration has been extended successfully");
    }
        @Override //overriding
        /**A method used to change the router model that I reserved before
        *@param reservationNumber give it the reservation number that I want to change the router model
         *@param router give it the new router details
         */
        public void changeRouterModel(int reservationNumber, Router router) {
        boolean found = false;
        for(Invoice invoice:invoices)
        {
            if(invoice.reservation.reservationID == reservationNumber)
            {
                found = true;
                invoice.reservation.setRouter(router);
            }
        }
        try {
            if(!found)
                throw new StringIndexOutOfBoundsException("Please enter a correct reservation number!");
        } catch (StringIndexOutOfBoundsException ex) {
            System.out.println("Error:" + ex.getMessage());
        }
    }
        @Override //overriding
        /**A method used to report an issue*/
        public void reportIssue(String message){
        this.reportIsuue = message;
        System.out.println("You reported the issue successfully");
    }
    
}




/**A child of CustomerBase class
 *
 */
 class resident extends CustomerBase{ //Inheritance
   
    public resident(String name) {
        super(name);
    }
    /**Rent Method used to make the resident rent a router
         *@param router give it the router details
         *@param type give it the type of renting{daily, monthly, weekly}
         *@param day give it the starting day 
         *@param month give it the starting month
         *@param year give it the starting year
         */    
    @Override 
    public void rent(Router router,String type,int startday, int startmonth, int startyear, int number){
        reservation = new Reservation(startday, startmonth, startyear);
        reservation.setRouter(router);
        reservation.setDueDate(type, number);
        invoice = new Invoice(reservation);
        invoice.setCustomerName(customerName);
        invoices.add(invoice);
        Invoice.isDiscount = true;
        System.out.println("Router has been rent successfully");
       };
}





/**A child of CustomerBase class
 *
 */
 class foreigner extends CustomerBase{ //Inheritance
     
    /**foreigner child class constructor*/
    public foreigner(String name) {
        super(name);
    }
    
/**Rent Method used to make the foreigner rent a router
     *@param router give it the router details
     *@param type give it the type of renting{daily, monthly, weekly}
     *@param day give it the starting day 
     *@param month give it the starting month
     *@param year give it the starting year
     */    
    @Override
    public void rent(Router router,String type, int day, int month, int year, int number){ //overloading
          reservation = new Reservation(day, month, year);
        reservation.setRouter(router);
        reservation.setDueDate(type, number);
        invoice = new Invoice(reservation);
        invoice.setCustomerName(customerName);
        invoices.add(invoice);
        Invoice.isDiscount = false;
        System.out.println("Router has been rent successfully");
    }
}





/** The class that represents Router characteristics. */
class Router {
    /**Router Serial Number*/
    public final String serial_num; //final data member
    private String model;
    private final int numOfPorts;
    public double price;
/**The router class constructor
 *@param model give it the model of the router
 *@param numOfPorts give it the number of ports of the router
 */
    public Router(String model, int numOfPorts) {
        serial_num = UUID.randomUUID().toString();
        this.model = model;
        this.numOfPorts = numOfPorts;
        
    }
    /**A method to display the router details*/
    public void display(){
       System.out.printf("\tSerail Number:%s \n\t Router Model:%s \n\t Number of Ports:%d \n\n",serial_num, model, numOfPorts);
    }  
    /**Model Getter
     *@return model
     */
    public String getModel() {
        return model;
    }
    /**Model Setter*/
    public void setModel(String model) {
        this.model = model;
    }
    /**Number of ports getter
     *@return numOfPorts
     */
    public int getNumOfPorts() {
        return numOfPorts;
    }
    
}





/**Reservation of routers Class*/
class Reservation{
    /**Reservation ID*/
    public int reservationID;
    private final Date reservationDate; //final data member
    private date startDate = new date();
    private date dueDate = new date();
    private date extendedDueDate = new date();
    public int numberOfRentingDays;
    /**Reserved router as data member of reservation class*/
    public Router router;
    private static int counter = 0; // static data member
    
/**Reservation Class Constructor
 *@param startday give it the starting day of reservation
 * @param startmonth give it the starting month of reservation
 * @param startyear give it the starting year of reservation
 */
    public Reservation(int startday, int startmonth, int startyear) {
        reservationDate = new Date();
        counter++;
        reservationID = counter;
        startDate.day = startday;
        startDate.month = startmonth;
        startDate.year = startyear;
        
    }
   /**Reservation Date getter
     *@return reservationDate
     */
    public final Date getReservationDate() { //final method
        return reservationDate;
    }
    /**Start Date getter
     *@return startDate
     */
    public date getStartDate() {
        return startDate;
    }
    /**Reserved Router getter
     *@return router
     */
    public Router getRouter() {
        return router;
    }
    /**Reserved router setter*/
    public void setRouter(Router router) {
        this.router = router;
    }
    /**A method to calculate Due Date
     *@param type give it to determine the type of renting (daily, monthly, weekly)
     * @param number give it to determine the number of reservation type
     */
    public void setDueDate(String type, int number) {
        switch (type) {
            case "daily":
                if(startDate.day + (1*number) < 30)
                {
                    dueDate.day = startDate.day+(1*number);
                    dueDate.month = startDate.month;
                    dueDate.year = startDate.year;
                }
                else
                {
                    dueDate.month = startDate.month+1;
                    dueDate.day = (startDate.day+(1*number))%30;
                    dueDate.year = startDate.year;
                }   numberOfRentingDays = 1*number;
                break;
            case "monthly":
                if(startDate.month + (1*number) < 12)
                {
                    dueDate.month = startDate.month+(1*number);
                    dueDate.day = startDate.day;
                    dueDate.year= startDate.year;
                }
                else
                {
                    dueDate.year = startDate.year+1;
                    dueDate.month = (startDate.month+(1*number))%12;
                    dueDate.day = startDate.day;
                }   numberOfRentingDays = 30*number;
                break;
            case "weekly":
                if(startDate.day + (7*number) < 30)
                {
                    dueDate.day = startDate.day+(7*number);
                    dueDate.month =startDate.month;
                    dueDate.year = startDate.year;
                }
                else
                {
                    dueDate.month = startDate.month+(7*number/30);
                    dueDate.day = (startDate.day+(7*number))%30;
                    dueDate.year = startDate.year;
                }   numberOfRentingDays = (7*number);
                break;
            default:
                break;
        }
    }
    /**Due Date getter
     *@return dueDate
     */
    public date getDueDate() {
        return dueDate;
    }
    /** A method to calculate extended due date*/
    public void extendedDueDate(String type, int number){
        
         switch (type) {
            case "daily":
                if(dueDate.day + (1*number) < 30)
                {
                    extendedDueDate.day = dueDate.day+(1*number);
                    extendedDueDate.month = dueDate.month;
                    extendedDueDate.year = dueDate.year;
                }
                else
                {
                    extendedDueDate.month = dueDate.month+1;
                    extendedDueDate.day = (dueDate.day+(1*number))%30;
                    extendedDueDate.year = dueDate.year;
                }   numberOfRentingDays = 1*number;
                break;
            case "monthly":
                if(dueDate.month + (1*number) < 12)
                {
                    extendedDueDate.month = dueDate.month+(1*number);
                    extendedDueDate.day = dueDate.day;
                    extendedDueDate.year= dueDate.year;
                }
                else
                {
                    extendedDueDate.year = dueDate.year+1;
                    extendedDueDate.month = (dueDate.month+(1*number))%12;
                    extendedDueDate.day = dueDate.day;
                }   numberOfRentingDays = 30*number;
                break;
            case "weekly":
                if(dueDate.day + (7*number) < 30)
                {
                    extendedDueDate.day = dueDate.day+(7*number);
                    extendedDueDate.month =dueDate.month;
                    extendedDueDate.year = dueDate.year;
                }
                else
                {
                    extendedDueDate.month = dueDate.month+(7*number/30);
                    extendedDueDate.day = (dueDate.day+(7*number))%30;
                    extendedDueDate.year = dueDate.year;
                }   numberOfRentingDays = (7*number);
                break;
            default:
                break;
        }
    }
    /**Extended Due Date getter
     *@return extendedDueDate
     */
    public date getExtendedDueDate() {
        return extendedDueDate;
    }
    
}






/**Invoice of renting routers Class*/
class Invoice{
    private String customerName;
    private double fees; //Calculated Date members
    /**The reservation that we want to calculate its invoice*/
    public Reservation reservation;
    public static boolean isDiscount; // static data member

    /**Invoice class constructor
     *@param reserve give it the reservation that we want to calculate its invoice
     */
    public Invoice(Reservation reserve){
        reservation = reserve;
    }
    /**Customer Name getter
     *@return CustomerName
     */
    public String getCustomerName() {
        return customerName;
    }
    /**Customer Name setter*/
    public void setCustomerName(String customerName) {
        this.customerName = customerName;
    }
    /**A method to calculate th fees of renting
     *@return fees
     */
    public double calculateFees(){
        switch (reservation.getRouter().getModel()) {
            case "Huawei310":
                fees = (reservation.numberOfRentingDays)*2;
                break;
            case "Huawei330":
                fees = (reservation.numberOfRentingDays)*3;
                break;
            case "Huawei350":
                fees = (reservation.numberOfRentingDays)*5;
                break;
            default:
                break;
        }
        if(isDiscount)
        {
            fees = fees - (fees*25/100);
        }
        System.out.println("#" + reservation.reservationID);
        System.out.println("Customer Name: " + customerName);
        System.out.println("Router Model: "+ reservation.getRouter().getModel());
        System.out.println("Number of Ports "+reservation.getRouter().getNumOfPorts());
        System.out.println("Start Date: "+reservation.getStartDate().day + "/" + reservation.getStartDate().month + "/" +reservation.getStartDate().year);
        System.out.println("Due Date: "+reservation.getDueDate().day+ "/" + reservation.getDueDate().month + "/" +reservation.getDueDate().year);
        System.out.println("Renting Fees: " + fees +  " pounds");
        return fees;
    }    
    /**Renting Fees getter
     *@return fees
     */
    public double getFees() {
        return fees;
    }
    /**A method to calculate the extended duration fees
     
     *@return fees
     */
    public double calculateExtendedFees(){
        switch (reservation.getRouter().getModel()) {
            case "Huawei310":
                fees = (reservation.numberOfRentingDays)*2;
                break;
            case "Huawei330":
                fees = (reservation.numberOfRentingDays)*3;
                break;
            case "Huawei350":
                fees = (reservation.numberOfRentingDays)*5;
                break;
            default:
                break;
        }
        if(isDiscount)
        {
            fees = fees - (fees*25/100);
        }
        System.out.println("#" + reservation.reservationID);
        System.out.println("Customer Name: " + customerName);
        System.out.println("Router Model: "+ reservation.getRouter().getModel());
        System.out.println("Number of Ports "+reservation.getRouter().getNumOfPorts());
        System.out.println("Start Date: "+reservation.getDueDate().day + "/" + reservation.getDueDate().month + "/" +reservation.getDueDate().year);
        System.out.println("Due Date: "+reservation.getExtendedDueDate().day+ "/" + reservation.getExtendedDueDate().month + "/" +reservation.getExtendedDueDate().year);
        System.out.println("Renting Fees: " + fees +  " pounds");
        return fees;
    }
}





/**Date class*/
class date
{
    protected int day;
    protected int month;
    protected int year;
}




/**Exception Class*/
class excep extends Exception
{
    public excep(String msg) //Exception Handling
    {
        super(msg);
    }
}
