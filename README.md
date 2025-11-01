# car-inventory-system
A Java based car inventory management system with JDBC database connectivity. Features include searching cars by make/model and calculating total value by year. Built with Java, Derby database and JDBC for efficient car data management and reporting.

package car2025paper;

import java.util.Scanner;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class Car2025Paper {

    public static void main(String[] args) throws SQLException {

        String dbURL="jdbc:derby://localhost:1527/Car2025";
        String username="app";
        String password="123";
        
        
        Connection conn=null;
        Statement stmt=null;
        ResultSet rs=null;

        
    //Establish connection to the database
    conn=DriverManager.getConnection(dbURL,username,password);
    
    // Create a Statement object    
       stmt=conn.createStatement();
    //Execute the SELECT * FROM CARS query
      rs=stmt.executeQuery("SELECT * FROM Cars");
    //Initialize Scanner f o r user input
     Scanner scanner = new Scanner(System.in) ;
     
     try{
        boolean running = true ;

        while ( running ) {
        //Display menu
            System.out.println("Menu: ") ;
            System.out.println( "1. Search Cars (by make/model) " ) ;
            System.out.println( "2. Total Value by Year ") ;
            System.out.println( " 3. Exit ") ;
            System.out.println( "Choose an option : " ) ;
            int choice = scanner.nextInt ( ) ;
            scanner.nextLine() ;    // consume newline
            
            // Execute the SELECT * FROM CARS query each time before calling a function
            
            rs = stmt.executeQuery( "SELECT * FROM CARS" ) ;

             switch ( choice ) {
                case 1:
                //Send the ResultSet to searchCars
                searchCars(rs);
                 break ;
                case 2:
                //Send the ResultSet to totalValueByYear
                    totalValueByYear(rs);
                 break ;
                case 3:
                running = false;
                 System. out.println("Exiting...") ;
                 break ;
                default :
                 System.out.println( " Invalid option . Please choose a valid option .") ;
                 break ;
                }
    
             System.out.println();
                }
            } catch ( Exception ex) {
                ex . printStackTrace ( ) ;
            }finally{
            //TODO: Close resources
            if(conn!=null)conn.close();
            if(stmt!=null)stmt.close();
            if(rs!=null)rs.close();
                    
            }
            scanner.close();
        }

    
        //Implement the searchCars and totalValueByYear functions here
        public static  void searchCars(ResultSet rs)
        {
        Scanner scanner=new Scanner(System.in);
        
            try{
                System.out.println("Enter the car that you want to seacher: ");
                String userInput=scanner.nextLine().toLowerCase();
                String data="";
                int counter=0;
                while(rs.next())
                {
                
                    String make=rs.getString("make");
                    String model=rs.getString("model");
                    String makeLC=make.toLowerCase();
                    String modelLC=model.toLowerCase();
                    
                   
                    if(makeLC.contains(userInput)|| modelLC.contains(userInput))
                    {
                        int id=rs.getInt("id");
                        int manufacture_year= rs.getInt("Manufacture_year");
                        double price=rs.getDouble("price");
                        
                        data+=id+" | "+make+" | "+model+" | "+manufacture_year+" | "+price+"\n";
                        counter++;
                    }
                }
                
                 System.out.println("Results:\nID | MAKE | MODEL | YEAR | PRICE");
                 System.out.println(data+"Maches found: "+counter);
                 
                
            }catch(SQLException ex)
            {
                System.out.println(ex.getMessage());
            }
        
        }
        
        public static void totalValueByYear(ResultSet rs)
        {
            Scanner scanner=new Scanner(System.in);
            
            int counter=0;
            String data="";
            double totalSum=0;
        try
        {
           System.out.println("Enter the manufacture year you want: ");
           int year=scanner.nextInt();
           
           while(rs.next())
           {
               
               int manufacture_year= rs.getInt("Manufacture_year");
               double price=rs.getDouble("price");
               
               if(manufacture_year == year)
               {
                   counter++;
                   totalSum+=price;
               }
           }
           data="For year "+year+":\nNumber of cars: "+counter+"\nTotal Value: "+totalSum;
           System.out.println(data);
            
        }catch(SQLException ex)
        {
            System.out.println(ex.getMessage());
        
        }
        
        
        }
}


